# Upload a custom server build to GameLift<a name="gamelift-build-cli-uploading"></a>

After you integrate your game server with GameLift, upload the build files so that GameLift can deploy it for game hosting\. This topic covers how to package your game's build files, create an optional build install script, and then upload the files using the [AWS Command Line Interface \(AWS CLI\)](http://aws.amazon.com/cli/) or an AWS SDK\.

## Add a build install script<a name="gamelift-build-cli-uploading-install"></a>

Create an install script for the operating system \(OS\) of your game build:
+ Windows: Create a batch file named `install.bat`\.
+ Linux: Create a shell script file named `install.sh`\.

When creating an install script, keep in mind the following:
+ The script can't take any user input\.
+ GameLift installs the build and recreates the file directories in your build package on a hosting server in the following locations:
  + Windows fleets: `C:\game`
  + Linux fleets: `/local/game`
+ During the installation process, the run\-as user has limited access to the instance file structure\. This user has full rights to the directory where your build files are installed\. If your install script performs actions that require administrator permissions, then specify admin access \(sudo for Linux, runas for Windows\)\. Permission failures related to the install script generate an event message that indicates a problem with the script\.
+ On Linux, GameLift supports common shell interpreter languages such as bash\. Add a shebang \(such as `#!/bin/bash`\) to the top of your install script\. To verify support for your preferred shell commands, remotely access an active Linux instance and open a shell prompt\. For more information, see [Remotely access GameLift fleet instances](fleets-remote-access.md)\.
+ The install script can't rely on a VPC peering connection\. A VPC peering connection isn't available until after GameLift installs the build on fleet instances\.

**Example Windows install bash file**  
This example `install.bat` file installs Visual C\+\+ runtime components required for the game server and writes the results to a log file\. The script includes the component file in the build package at the root\.  

```
vcredist_x64.exe /install /quiet /norestart /log c:\game\vcredist_2013_x64.log
```

**Example Linux install shell script**  
This example `install.sh` file uses bash in the install script and writes results to a log file\.  

```
#!/bin/bash
echo 'Hello World' > install.log
```
This example `install.sh` file shows how you can use the Amazon CloudWatch agent to collect system\-level and custom metrics, and handle log rotation\. Because GameLift runs in a service VPC, you must grant GameLift permissions to assume an AWS Identity and Access Management \(IAM\) role on your behalf\. To allow GameLift to assume a role, create a role that includes the AWS managed policy `CloudWatchAgentAdminPolicy`, and use that role when you create a fleet\.  

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
            // For more information, see [Manually create or edit the CloudWatch agent configuration file](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html).
        }
    }
}
EOF
sudo mv /tmp/config.json /opt/aws/amazon-cloudwatch-agent/bin/config.json
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json
sudo systemctl enable amazon-cloudwatch-agent.service
```

## Package your game build files<a name="gamelift-build-packaging"></a>

Before uploading your configured game server to GameLift for hosting, package the game build files into a build directory\. This directory must include all components required to run your game servers and host game sessions, including the following:
+ **Game server binaries** – The binary files required to run the game server\. A build can include binaries for multiple game servers built to run on the same platform\. For a list of supported platforms, see [Download Amazon GameLift SDKs](gamelift-supported.md)\.
+ **Dependencies** – Any dependent files that your game server executables require to run\. Examples include assets, configuration files, and dependent libraries\.
+ **Install script** – A script file to handle tasks that are required to fully install your game build on GameLift hosting servers\. Place this file at the root of the build directory\. GameLift runs the install script as part of fleet creation\.

You can set up any application in your build, including your install script, to access your resources securely on other AWS services\. For information about ways to do this, see [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.

After you've packaged your build files, make sure that your game server can run on a clean installation of your target OS\. This step helps ensure that you include all required dependencies in your package and that your install script is accurate\.



## Create a GameLift build<a name="gamelift-build-cli-uploading-builds"></a>

When creating a build and uploading your files, you have a couple of options:
+ [Create a build from a file directory](#gamelift-build-cli-uploading-upload-build)\. This is the simplest and most commonly used method\.
+ [Create a build with files in Amazon Simple Storage Service \(Amazon S3\)](#gamelift-build-cli-uploading-create-build)\. With this option, you can manage your build versions in Amazon S3\.

With both methods, GameLift creates a new build resource with a unique build ID and other metadata\. The build starts in the **Initialized** status\. After GameLift acquires the game server files, the build moves to **Ready** status\. At this point, you can deploy it by creating a new GameLift fleet\. For more information, see [Deploy a fleet](fleets-creating.md)\.

When GameLift sets up the new fleet, it downloads the build files to each fleet instance and installs the build file based on the build install script\. GameLift installs build files on the instances in the following locations:
+ Windows fleets: `C:\game`
+ Linux fleets: `/local/game`

### Create a build from a file directory<a name="gamelift-build-cli-uploading-upload-build"></a>

To create a game build with packaged game server files stored in any location, including a local directory, use the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html) AWS CLI command\. This command creates a new build record in GameLift and uploads files from a location that you specify\.

1. **Send an upload request\.** In a command line window, enter the following upload\-build command and parameters\.

   ```
   aws gamelift upload-build \
       --operating-system supported OS \
       --build-root build path \
       --name user-defined name of build \
       --build-version user-defined build number \
       --region region name
   ```
   + operating\-system – The game server build's OS\. If you don't specify an OS, GameLift uses the default value \(`WINDOWS_2012`\)\. You can't update this later\.
   + build\-root – The directory path of your build files\.
   + name – A descriptive name for the new build\. You can update this value at any time by updating the build resource\.
   + build\-version – The version details for the build files\.
   + region – The AWS Region where you want to create your build\. Create the build in the Region where you plan to deploy fleets\. If you're deploying your game in multiple Regions, create a build in each Region\.
**Note**  
Check your current default Region using the [https://docs.aws.amazon.com/cli/latest/reference/configure/get.html](https://docs.aws.amazon.com/cli/latest/reference/configure/get.html) command \(aws configure get region\)\. To change your default Region, use the [https://docs.aws.amazon.com/cli/latest/reference/configure/set.html](https://docs.aws.amazon.com/cli/latest/reference/configure/set.html) command \(aws configure set region *region name*\)\.

   *Examples*

   ```
   aws gamelift upload-build \
       --operating-system AMAZON_LINUX \
       --build-root "~/mygame" \
       --name "My Game Nightly Build" \
       --build-version "build 255" \
       --region us-west-2
   ```

   ```
   aws gamelift upload-build \
       --operating-system WINDOWS_2012 \
       --build-root "C:\mygame" \
       --name "My Game Nightly Build" \
       --build-version "build 255" \
       --region us-west-2
   ```

   In response to your upload request, GameLift provides upload progress\. On a successful upload, GameLift returns the new build record ID\. Upload time depends on the size of your game files and the connection speed\.

1. **Check build status\.** View the new build record, including the current status, using the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-build.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-build.html) command \(or the [DescribeBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeBuild.html) API operation\)\. You can also view the status in the GameLift console\.

   In a command line window, enter the following describe\-build command and parameters\.

   ```
   aws gamelift describe-build \
       --build-id build ID returned with the upload request \
       --region region name
   ```

   *Example*

   ```
   aws gamelift describe-build \
       --build-id "build-3333cccc-44dd-55ee-66ff-7777aaaa88bb" \
       --region us-west-2
   ```

   In response to your request, GameLift returns the requested build record\. This record contains a set of build metadata including the status, the size of the uploaded files, and a creation timestamp\.

### Create a build with files in Amazon S3<a name="gamelift-build-cli-uploading-create-build"></a>

You can store your build files in Amazon S3 and upload them to GameLift from there\. When you create you build, you specify the S3 bucket location, and GameLift retrieves the build files directly from Amazon S3\.

**To create a build resource**

1. **Store your build files in Amazon S3\.** Create a \.zip file containing the packaged build files and upload it to an S3 bucket in your AWS account\. Take note of the bucket label and the file name—you'll need these when creating a GameLift build\.

1. **Give GameLift access to your build files\.** Create an IAM role by following the instructions in [Access a game build file in Amazon S3](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-access-storage-loc)\. Your IAM role specifies which entities \(such as GameLift\) can assume the role\. It also defines a set of permissions for limited access to your AWS resources\. After you've created the role, take note of the new role's Amazon Resource Name \(ARN\), which you'll need when creating a build\.

1. **Create a build\.** Use the GameLift console or the AWS CLI to create a new build record\. You must have the `PassRole` permission, as described in [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\.

------
#### [ GameLift console ]

1. In the [GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Hosting**, **Builds**\.

1. On the **Builds** page, choose **Create build**\.

1. On the **Create build** page, under **Build settings**, do the following:

   1. For **Name**, enter a script name\.

   1. For **Version**, enter a version\. Because you can update the content of a build, version data can help you track updates\.

   1. \(Optional\) For **Operating system \(OS\)**, choose the OS of your game server\. By default, GameLift uses Windows 2012\. You can't update the OS later\.

   1. For **Game server build**, enter the **S3 URI** of the build object that you uploaded to Amazon S3, and choose the **Object version**\. If you don't remember the Amazon S3 URI and object version, choose **Browse S3** and search for the build object\.

   1. For **IAM role**, choose the role that you created that gives GameLift access to your S3 bucket and build object\.

1. \(Optional\) Under **Tags**, add tags to the build by entering **Key** and **Value** pairs\.

1. Choose **Create**\.

   GameLift assigns an ID to the new build and uploads the designated \.zip file\. You can view the new build, including the status, on the **Builds** page\.

------
#### [ AWS CLI ]

To define the new build and upload your server build files, use the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html) command\.

1. Open a command line window and switch to a directory where you can use the AWS CLI\.

1. Enter the following create\-build command:

   ```
   aws gamelift create-build \
       --name user-defined name of build \
       --operating-system supported OS \
       --build-version user-defined build number \
       --storage-location "Bucket"=S3 bucket label,"Key"=Build .zip file name,"RoleArn"=Access role ARN} \
       --region region name
   ```
   + name – A descriptive name for the new build\.
   + operating\-system – The game server build's OS\. If you don't specify an OS, GameLift uses the default value \(`WINDOWS_2012`\)\. You can't update this later\.
   + build\-version – The version details for the build files\. This information can be useful because each new version of your game server requires a new build resource\.
   + storage\-location
     + Bucket – The name of the S3 bucket that contains your build\. For example, "my\_build\_files"\.
     + Key – The name of the \.zip file that contains your build files\. For example, "my\_game\_build\_7\.0\.1, 7\.0\.2"\.
     + RoleARN – The ARN assigned to the IAM role that you created\. For example, "arn:aws:iam::111122223333:role/GameLiftAccess"\. For an example policy, see [Access a game build file in Amazon S3](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-access-storage-loc)\.
   + region – Create the build in the AWS Region where you plan to deploy fleets\. If you're deploying your game in multiple Regions, create a build in each Region\.
**Note**  
We recommend checking your current default Region using the [https://docs.aws.amazon.com/cli/latest/reference/configure/get.html](https://docs.aws.amazon.com/cli/latest/reference/configure/get.html) command[https://docs.aws.amazon.com/cli/latest/reference/configure/get.html](https://docs.aws.amazon.com/cli/latest/reference/configure/get.html)\. To change your default Region, use the [https://docs.aws.amazon.com/cli/latest/reference/configure/set.html](https://docs.aws.amazon.com/cli/latest/reference/configure/set.html) command\.

   *Example*

   ```
   aws gamelift create-build \
       --operating-system WINDOWS_2012 \
       --storage-location "Bucket"="my_game_build_files","Key"="mygame_build_101.zip","RoleArn"="arn:aws:iam::111122223333:role/gamelift" \
       --name "My Game Nightly Build" \
       --build-version "build 101" \
       --region us-west-2
   ```

1. To view the new build, use the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-build.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-build.html) command\.

------

## Update your build files<a name="gamelift-build-cli-uploading-update-build-files"></a>

You can update the metadata for a build resource using the GameLift console or the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-build.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-build.html) AWS CLI command\. After you've created a GameLift build, you can't update the build files associated with it\. For each new set of files, create a new GameLift build\. Using the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html) command, GameLift automatically creates a new build record for each request\. If you provide build files using the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html) command, upload a new build \.zip file with a different name to Amazon S3 and create a build by referencing the new file name\.

Try these tips for deploying updated builds:
+ **Use queues and swap out fleets as needed\.** When setting up your game client with GameLift, specify a queue instead of a fleet\. With queues, you can add the new fleets with the new build to your queue and remove the old fleets\. For more information, see [Setting up GameLift queues for game session placement](queues-intro.md)\.
+ **Use aliases to transfer players to a new game build\.** When integrating your game client with GameLift, specify a fleet alias instead of a fleet ID\. For more information, see [Add an alias to a GameLift fleet](aliases-creating.md)\.
+ **Set up automated build updates\.** For sample scripts and information about incorporating GameLift deployments into your build system, see [Automating Deployments to Amazon GameLift](http://aws.amazon.com/blogs/gametech/automating-deployments-to-amazon-gamelift/) on the AWS Game Tech Blog\.