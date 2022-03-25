# Upload a custom server build to GameLift<a name="gamelift-build-cli-uploading"></a>

Once your game server has been integrated with GameLift, upload the build files to the managed GameLift service so that it can be deployed for game hosting\. This topic covers how to package your game's build files, create an optional build install script, and then upload the files using either the AWS CLI or the AWS SDK\. 

## Add a build install script<a name="gamelift-build-cli-uploading-install"></a>

Create an install script for the operating system of your game build: 
+ Windows: create a batch file named "`install.bat`"\. 
+ Linux: create a shell script file named "`install.sh`"\.

When creating an install script, keep in mind the following:
+ The script cannot take any user input\. 
+ A build is installed on a hosting server in the following locations\. File directories in your build package are recreated\.
  + For Windows fleets: `C:\game`
  + For Linux fleets: `/local/game`
+ During the installation process, the run\-as user has limited access to the instance file structure\. It has full rights to the directory where your build files are installed\. If your install script takes actions that require administrator permissions, you'll need to specify admin access \(`sudo` for Linux, `runas` for Windows\)\. Permission failures related to the install script generate an event message that indicates a problem with the script\.
+ On Linux, common shell interpreter languages such as bash are supported\. Add a shebang \(such as `#!/bin/bash`\) to the top of your install script\. If you need to verify support for your preferred shell commands, you can remotely access an active Linux instance and open a shell prompt\. Learn more at [Remotely access GameLift fleet instances](fleets-remote-access.md)\.
+ The install script cannot rely on a VPC peering connection\. If you're planning to set up VPC peering when creating fleets for this build, this connection is not available until after the build is installed on fleet instances\.

**Example scripts**  
These examples illustrate common script usage for Windows and Linux\.

**Windows**

This example install\.bat file installs Visual C\+\+ runtime components required for the game server and writes the results to a log file\. The component file is included in the build package at the root\.

```
vcredist_x64.exe /install /quiet /norestart /log c:\game\vcredist_2013_x64.log
```

**Linux**

This example install\.sh file illustrates using bash in your install script and writing results to a log file\.

```
#!/bin/bash
echo 'Hello World' > install.log
```

This example install\.sh file shows how you can use the Amazon CloudWatch agent to collect system\-level metrics, custom metrics, and handle log rotation\. 

```
sudo yum install -y amazon-cloudwatch-agent
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install -y collectd
cat <<'EOF' > /tmp/config.json
{
    "agent": {
        "metrics_collection_interval": 60,
        "run_as_user": "root",
        "credentials": {
            "role_arn": "arn:aws:iam::account#:role/rolename"
        }
    },
    "logs": {
        "logs_collected": {
            "files": {
                "collect_list": [
                    {
                        "file_path": "/tmp/log",
                        "log_group_name": "gllog",
                        "log_stream_name": "{instance_id}"
                    }
                ]
            }
        }
    },
    "metrics": {
       "namespace": "GL_Metric",
        "append_dimensions": {
            "ImageId": "${aws:ImageId}",
            "InstanceId": "${aws:InstanceId}",
            "InstanceType": "${aws:InstanceType}"
        },
        "metrics_collected": {
            // Configure metrics you want to collect.
            // For more information, see [ Manually create or edit the CloudWatch agent configuration file](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html).
        }
    }
}
EOF
sudo mv /tmp/config.json /opt/aws/amazon-cloudwatch-agent/bin/config.json
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
sudo systemctl enable amazon-cloudwatch-agent.service
```

Since GameLift is running in a service VPC, you must grant permissions for GameLift to assume the role on your behalf\. You must create a role that includes `CloudWatchAgentAdminPolicy` and use the role when you create a fleet\. 

## Package your game build files<a name="gamelift-build-packaging"></a>

Before uploading your GameLift\-enabled game server to the GameLift service for hosting, you need to package the game build files into a build directory\. This directory must include all components required to run your game servers and host game sessions, including the following: 
+ **Game server binaries** – The binary files required to run the game server\. A build can include binaries for multiple game servers, as long as they are built to run on the same platform \(see [supported platforms](gamelift-supported.md)\)\.
+ **Dependencies** – Any dependent files required by your game server executables to run\. Examples include assets, configuration files, and dependent libraries\.
+ **Install script** – Script file to handle tasks that are required to fully install your game build onto GameLift hosting servers\. This file must be placed at the root of the build directory\. An install script is run once as part of fleet creation\. 

You can set up any application in your build, including your install script, to securely access your resources on other AWS services\. Learn more about possible ways to do this in [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.

Once you've packaged your build files, make sure your game server can run on a clean installation of your target operating system \(not one that's been used for development\)\. This step helps ensure that you include all required dependencies in your package and that your install script is accurate\.

**Note**  
If you're storing your game build files in an Amazon S3 bucket for uploading, you need to package the build files into a `.zip` file\. See instructions for uploading using this method in [Create a build with files in Amazon S3 ](#gamelift-build-cli-uploading-create-build)\. 



## Create a GameLift build<a name="gamelift-build-cli-uploading-builds"></a>

When creating a build and uploading your files, you have a couple of options: 
+ [Create a build from a file directory](#gamelift-build-cli-uploading-upload-build)\. This is the simplest and most commonly used method\.
+ [Create a build with files in Amazon S3 ](#gamelift-build-cli-uploading-create-build)\. With this option, you can manage your build versions in S3 under your AWS account\.

With both methods, a new build resource is created with a unique build ID and other metadata\. The build is placed in **Initialized** status\. Once GameLift successfully acquires the game server files, the build moves to **Ready** status\. At this point, you can deploy it by creating a new GameLift fleet \(see [Deploy a GameLift fleet with a custom game build](fleets-creating.md)\)\.

When you create a fleet, you specify which build to deploy to the fleet\. When GameLift sets up the new fleet, it downloads the build files to each fleet instance, and installs it based on the build install script \(if one is provided\)\. Build files are installed on the instances in the following locations:
+ For Windows fleets: `C:\game`
+ For Linux fleets: `/local/game`

### Create a build from a file directory<a name="gamelift-build-cli-uploading-upload-build"></a>

To create a game build with packaged game server files stored in any location, including a local directory, use the AWS Command Line Interface \(AWS CLI\) command [upload\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html)\. This command creates a new build record in GameLift and uploads files from a location that you specify\.

1. **Send an upload request\.** In a command line window, enter the following command and parameters\. 

   ```
   AWS gamelift upload-build --operating-system [supported OS] --build-root [build path] --name [user-defined name of build] --build-version [user-defined build number] --region [region name]
   ```
   + Build root – The directory path of your build files\.
   + Operating system – Specify the game server build's OS\. When a new fleet is created for this build, fleet instances are configured with the appropriate OS\. GameLift supports several Windows and Linux varieties\. This parameter is optional\. If an operating system is not specified, GameLift uses the default value \(`WINDOWS_2012`\)\. Once the build is created, this value cannot be updated later\.
   + Name – Provide a descriptive name for the new build\. Build name does not need to be unique, and you can update this value at any time by updating the build resource\.
   + Build version – Use this optional field to specify version details for the build files\. Since each new version of your game server requires a new build resource, this information can provide a valuable differentiator\.
   + Region – Identify the GameLift\-supported region where you want to create your build\. You must create the build in the region where you plan to deploy fleets\. If you're deploying your game in multiple regions, you must create a build in each region\. 
**Note**  
If you work in multiple regions, it is always a good idea to check your current default region\. In the AWS console, the current region is always displayed in the upper right corner, with a dropdown list of available regions to select from\. If you're using the AWS CLI, check your current default using the [configure get](https://docs.aws.amazon.com/cli/latest/reference/configure/get.html) \(`AWS configure get region`\)\. Use the command [configure set](https://docs.aws.amazon.com/cli/latest/reference/configure/set.html) \(`AWS configure set region [region name]`\) to change your default region\.

   In response to your upload request, the GameLift service provides upload progress, and on a successful upload returns the new build record ID\. Upload time depends on the size of your game files and the connection speed\.

   *Examples*:

   ```
   AWS gamelift upload-build --operating-system AMAZON_LINUX --build-root ~/mygame --name "My Game Nightly Build" --build-version "build 255" --region us-west-2
   ```

   ```
   AWS gamelift upload-build --operating-system WINDOWS_2012 --build-root "C:\mygame" --name "My Game Nightly Build" --build-version "build 255" --region us-west-2
   ```

1. **Check build status\.** View the new build record, including current status, using [describe\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-build.html) \(or [DescribeBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeBuild.html)\)\. You can also view status on the GameLift console\.

   In a command line window, type the following command and parameters\.

   ```
   AWS gamelift describe-build --build-id [build ID returned with the upload request] --region [region name]
   ```

   *Example*:

   ```
   AWS gamelift describe-build --build-id "build-3333cccc-44dd-55ee-66ff-7777aaaa88bb" --region us-west-2
   ```

   In response to your request, the GameLift service returns the requested build record\. This record contains a set of build metadata including status, size of the uploaded files, and a creation time stamp\. 

### Create a build with files in Amazon S3<a name="gamelift-build-cli-uploading-create-build"></a>

To create a game build with packaged game server files that are stored in an Amazon S3 bucket under your AWS account, use the AWS CLI command [create\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html)\. This operation creates a new build in GameLift and acquires your build files from the Amazon S3 bucket that you specify\. GameLift will report the `SizeOnDisk` as 0\. The size of the file can be found in Amazon S3\. 

1. **Store your build files in Amazon S3\.** Create a `.zip` file containing the packaged build files and upload it to an Amazon S3 bucket under your AWS account\. Take note of the bucket label and the file name; you'll need these when creating a GameLift build\.

1. **Give GameLift access to your build files\.** Follow the instructions at [Set up a role for GameLift access](setting-up-role.md) to create an IAM role\. A role specifies which entities \(such as the GameLift service\) can assume the role and defines a set of permissions for limited access to your AWS resources\. Once you've created the role, take note of the new role's Amazon Resource Name \(ARN\), which you'll need when creating a build\. 

1. **Send a request to create a new build\.** Use the AWS CLI command [create\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html) \(or the AWS SDK operation [CreateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateBuild.html)\) to create a new build record and tell GameLift where your build files are stored\. You must have IAM PassRole permission, as described in [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\. 

   In this request, specify an Amazon S3 location, including the following information, which you collected when setting up your bucket and access role\. Enter the following command and parameters\.

   ```
   AWS gamelift create-build --name [user-defined name of build] --operating-system [supported OS] --build-version [user-defined build number] --storage-location "Bucket=[S3 bucket label],Key=[Build zip file name],RoleArn=[Access role ARN]" --region [region name]
   ```
   + Name – Provide a descriptive name for the new build\. Build name does not need to be unique, and you can update this value at any time by updating the build resource\.
   + Operating system – Specify the game server build's OS\. When a new fleet is created for this build, fleet instances are configured with the appropriate OS\. GameLift supports several Windows and Linux varieties\. This parameter is optional\. If an operating system is not specified, GameLift uses the default value \(`WINDOWS_2012`\)\. Once the build is created, this value cannot be updated later\.
   + Build version – Use this optional field to specify version details for the build files\. Since each new version of your game server requires a new build resource, this information can provide a valuable differentiator\.
   + S3 location
     + Bucket – Name of the S3 bucket that contains your build\. Example: "my\_build\_files"\.
     + Key – Name of the `.zip` file that contains your build files\. Example: "my\_game\_build\_7\.0\.1, 7\.0\.2"\. 
   + Role ARN – ARN assigned to the IAM role that you created\. Example: "arn:aws:iam::111122223333:role/GameLiftAccess"\. For an example policy, see [Access a game build file in Amazon S3](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-access-storage-loc)\. 
   + Region – Identify the GameLift\-supported region where you want to create your build\. You must create the build in the region where you plan to deploy fleets\. If you're deploying your game in multiple regions, you must create a build in each region\. 
**Note**  
It is always a good idea to check your current default AWS Region\. In the AWS console, the current Region is displayed in the upper right corner with a dropdown list of available regions to select from\. If you're using the AWS CLI, check your current default using [configure get](https://docs.aws.amazon.com/cli/latest/reference/configure/get.html) \(`AWS configure get region`\)\. Use the command [configure set](https://docs.aws.amazon.com/cli/latest/reference/configure/set.html) \(`AWS configure set region [region name]`\) to change your default Region\.

   *Example*:

   ```
   AWS gamelift create-build --operating-system WINDOWS_2012 --storage-location "Bucket=my_game_build_files,Key=mygame_build_101.zip,RoleArn=arn:aws:iam::111122223333:role/gamelift" --name "My Game Nightly Build" --build-version "build 101" --region us-west-2
   ```

   In response to your request, the GameLift service returns the newly created build record, including the build's current status\.

## Update your build files<a name="gamelift-build-cli-uploading-update-build-files"></a>

Once a GameLift build has been created, the build files associated with it cannot be changed\. Instead, you must create a new GameLift build for each new set of files\. If you provide build files using the `upload-build` command, you don't need to do anything special because GameLift automatically creates a new build record for each request\. If you provide build files using the `create-build` command, upload a new build `.zip` file with a different name to Amazon S3 and create a build by referencing the new file name\. 

Try these tips for deploying updated builds:
+ **Use queues and swap out fleets as needed\.** When setting up your game client with GameLift, specify a queue instead of a fleet\. With queues, you can create new fleets to run your new build, then add the new fleets to your queue and remove the old fleets\. For details , see [Setting up GameLift queues for game session placement](queues-intro.md)\.
+ **Use aliases to silently transfer players to a new game build\.** When integrating your game client with GameLift, specify a fleet alias instead of a fleet ID\. With aliases, you can move players to a new build in just three steps: \(1\) Create the new build\. \(2\) Create a fleet to deploy the new build\. \(3\) Change the alias target from the old fleet to the new fleet\. For more information, see [Add an alias to a GameLift fleet](aliases-creating.md)\. 
+ **Set up automated build updates\. **Follow the GameTech blog post [ Automating deployments to GameLift](https://aws.amazon.com/blogs/gametech/automating-deployments-to-amazon-gamelift/), with sample scripts, to incorporate GameLift deployments into your build system\.