# Upload a Custom Game Server Build to Amazon GameLift<a name="gamelift-build-cli-uploading"></a>

When you have a game server build that's been integrated with Amazon GameLift, you need to upload the build to the Amazon GameLift service before it can be deployed for game hosting\. 

Before uploading the files, first package the build files for your game\. Optionally, include a build install script\. Once the files are ready, use either the AWS CLI or the AWS SDK to create an Amazon GameLift build and upload the build files\. There are two possible ways to accomplish this: 
+ Create a build with files stored in a directory\. This is the simplest and most commonly used method\.
+ Create a build with files stored in an Amazon S3 bucket under your AWS account\.\.

When you create a build, a new game build record is created\. This record contains a unique build ID \(example: `build-1111aaaa-22bb-33cc-44dd-5555eeee66ff`\), creation time stamp, uploaded file size, status, and other metadata\. The build is placed in **Initialized** status\. Once Amazon GameLift successfully acquires the game server files, the build moves to **Ready** status\. At this point, you can deploy it by creating a new Amazon GameLift fleet \(see [Deploy a GameLift Fleet for a Custom Game Build](fleets-creating.md)\)\.

When you create a fleet for the build, Amazon GameLift sets up new fleet instances and installs the game server build on each instance\. Build files are installed in the following locations:
+ For Windows fleets: `C:\game`
+ For Linux fleets: `/local/game`

To troubleshoot fleet activation problems that might be related to build installation, see [Debug Fleet Issues](fleets-creating-debug.md)\.

## Create a Build Install Script<a name="gamelift-build-cli-uploading-install"></a>

Create an install script for the operating system of your game build: 
+ Windows: create a batch file named "`install.bat`"\. 
+ Linux: create a shell script file named "`install.sh`"\.

When creating an install script, keep in mind the following:
+ The script cannot take any user input\. 
+ A build is installed on a hosting server in the following locations\. File directories in your build package are recreated\.
  + For Windows fleets: `C:\game`
  + For Linux fleets: `/local/game`
+ During the installation process, the run\-as user has limited access to the instance file structure\. It has full rights to the directory where your build files are installed\. If your install script takes actions that require administrator permissions, you'll need to specify admin access \(`sudo` for Linux, `runas` for Windows\)\. Permission failures related to the install script generate an event message indicating a problem with the script\.
+ On Linux, common shell interpreter languages such as bash are supported\. Add a shebang \(such as `#!/bin/bash`\) to the top of your install script\. If you need to verify support for your preferred shell commands, you can remotely access an active Linux instance and open a shell prompt\. Learn more at [Remotely Access Fleet Instances](fleets-remote-access.md)\.

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

## Package Your Game Build Files<a name="gamelift-build-packaging"></a>

Before uploading your Amazon GameLift\-enabled game server to the Amazon GameLift service for hosting, you need to package the game build files into a build directory\. This directory must include all components required to run your game servers and host game sessions, including the following: 
+ **Game server binaries** – The binary files required to run the game server\. A build can include binaries for multiple game servers, as long as they are built to run on the same platform \(see [supported platforms](gamelift-supported.md)\)\.
+ **Dependencies** – Any dependent files required by your game server executables to run\. Examples include assets, configuration files, and dependent libraries\.
+ **Install script** – Script file to handle tasks that are required to fully install your game build onto Amazon GameLift hosting servers\. This file must be placed at the root of the build directory\. An install script is run once as part of fleet creation\. 

You can set up any application in your build, including your install script, to securely access your resources on other AWS services\. Learn more about possible ways to do this in [Access AWS Resources From Your Fleets](gamelift-sdk-server-resources.md)\.

Once you've packaged your build files, make sure your game server can run on a clean installation of your target operating system \(not one that's been used for development\)\. This step helps ensure that you include all required dependencies in your package and that your install script is accurate\.

**Note**  
If you're storing your game build files in an Amazon S3 bucket for uploading, you need to package the build files into a `.zip` file\. See instructions for uploading using this method in [Create a Build with Files in Amazon S3 ](#gamelift-build-cli-uploading-create-build)\. 

## Create a Build with Files in a Directory<a name="gamelift-build-cli-uploading-upload-build"></a>

To create a game build with packaged game server files stored in any location, including a local directory, use the AWS Command Line Interface \(AWS CLI\) command [upload\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html)\. This command creates a new build record in Amazon GameLift and uploads files from a location you specify\. 

1. **Send an upload request\.** In a command line window, type the following command and parameters\. 

   ```
   aws gamelift upload-build --operating-system [supported OS] --build-root [build path] --name [user-defined name of build] --build-version [user-defined build number] --region [region name]
   ```  
`--build-root`  
The directory path of your build files\.  
`--operating-system`  
The operating system that all server executables in the build run on\. All fleets created with this build automatically use this OS\. Valid values are `WINDOWS_2012` and `AMAZON_LINUX`\. This parameter is optional\. If an operating system is not specified, Amazon GameLift uses the default value \(`WINDOWS_2012`\)\. This value cannot be changed later\.  
`--region`  
Name of the region that you want to create your build in\. You must create the build in the region that you want to deploy fleets in\. To deploy in multiple regions, you must create a build and upload files for each region\. If you configured your AWS CLI with a default region, you can omit this parameter\.   
If you work in multiple regions, it is always a good idea to check your current default region\. Use the AWS CLI command [configure get](https://docs.aws.amazon.com/cli/latest/reference/configure/get.html) \(`aws configure get region`\) to see your current default region\. Use the command [configure set](https://docs.aws.amazon.com/cli/latest/reference/configure/set.html) \(`aws configure set region [region name]`\) to set a default region\.  
`--name` and `--build-version`   
Use these parameters to describe your build\. These metadata values can be changed later using [update\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-build.html) \(or the AWS SDK operation [UpdateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateBuild.html)\)\.

   In response to your upload request, the Amazon GameLift service provides upload progress, and on a successful upload returns the new build record ID\. Upload time depends on the size of your game files and the connection speed\.

   *Examples*:

   ```
   aws gamelift upload-build --operating-system AMAZON_LINUX --build-root ~/mygame --name "My Game Nightly Build" --build-version "build 255" --region us-west-2
   ```

   ```
   aws gamelift upload-build --operating-system WINDOWS_2012 --build-root "C:\mygame" --name "My Game Nightly Build" --build-version "build 255" --region us-west-2
   ```

1. **Check build status\.** View the new build record, including current status, using [describe\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-build.html) \(or [DescribeBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeBuild.html)\)\. You can also view status on the Amazon GameLift console\.

   In a command line window, type the following command and parameters\.

   ```
   aws gamelift describe-build --build-id [build ID returned with the upload request] --region [region name]
   ```

   *Example*:

   ```
   aws gamelift describe-build --build-id "build-3333cccc-44dd-55ee-66ff-7777aaaa88bb" --region us-west-2
   ```

   In response to your request, the Amazon GameLift service returns the requested build record\. This record contains a set of build metadata including status, size of the uploaded files, and a creation time stamp\. 

## Create a Build with Files in Amazon S3<a name="gamelift-build-cli-uploading-create-build"></a>

To create a game build with packaged game server files stored in an Amazon S3 bucket in your AWS account, use the AWS CLI command [create\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html)\. This operation creates a new build in Amazon GameLift and acquires your build files from the Amazon S3 bucket that you specify\. 

1. **Store your build files in Amazon S3\.** Create a `.zip` file containing the packaged build files and upload it to an Amazon S3 bucket in your AWS account\. Take note of the bucket label and the file name; you'll need these when creating an Amazon GameLift build\.

1. **Give Amazon GameLift access to your build files\.** Follow the instructions at [Set Up a Role for Amazon GameLift Access](setting-up-role.md) to create an IAM role\. A role lays out a set of permissions for limited access to your AWS resources and specifies which entities \(such as the Amazon GameLift service\) can assume the role\. Once you've created the role, take note of the new role's Amazon Resource Name \(ARN\), which you'll need when creating a build\. 

1. **Send a request to create a new build\.** Use the AWS CLI command [create\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-build.html) \(or the AWS SDK operation [CreateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateBuild.html)\) to create a new build record and tell Amazon GameLift where your build files are stored\. In this request, you must specify an Amazon S3 location, including the following information \(which you collected when setting up your bucket and access role\):
   + Bucket: The name of the bucket that contains your build\. Example: "my\_build\_files"\.

      
   + Key: The name of the `.zip` file that contains your build files\. Example: "mygame\_build\_7\.0\.1, 7\.0\.2"\. 

      
   + Role ARN: The ARN assigned to the access role you created\. Example: "arn:aws:iam::111122223333:role/GameLiftAccess"\.

   In a command line window, type the following command and parameters\.

   ```
   aws gamelift create-build --operating-system [supported OS] --storage-location "Bucket=[S3 bucket label],Key=[Build zip file name],RoleArn=[Access role ARN]" --name [user-defined name of build] --build-version [user-defined build number] --region [region name]
   ```

   *Example*:

   ```
   aws gamelift create-build --operating-system WINDOWS_2012 --storage-location "Bucket=my_game_build_files,Key=mygame_build_101.zip,RoleArn=arn:aws:iam::111122223333:role/gamelift" --name "My Game Nightly Build" --build-version "build 101" --region us-west-2
   ```

   In response to your request, the Amazon GameLift service returns the newly created build record\.

## Update Your Build Files<a name="gamelift-build-cli-uploading-update-build-files"></a>

Once an Amazon GameLift build has been created, the build files associated with it cannot be changed\. Instead, you must create a new Amazon GameLift build for each new set of files\. If you provide build files using the `upload-build` command, you don't need to do anything special because Amazon GameLift automatically creates a new build record for each request\. If you provide build files using the `create-build` command, upload a new build `.zip` file with a different name to Amazon S3 and create a build by referencing the new file name\. 

Try these tips for deploying updated builds:
+ **Use queues and swap out fleets as needed\.** When setting up your game client with Amazon GameLift, specify a queue instead of a fleet\. With queues, you can create new fleets to run your new build, then add the new fleets to your queue and remove the old fleets\. For details , see [Using Multi\-Region Queues](queues-intro.md)\.
+ **Use aliases to silently transfer players to a new game build\.** When integrating your game client with Amazon GameLift, specify a fleet alias instead of a fleet ID\. With aliases, you can move players to a new build in just three steps: \(1\) Create the new build\. \(2\) Create a fleet to deploy the new build\. \(3\) Change the alias target from the old fleet to the new fleet\. For more information, see [Add an Alias to a Amazon GameLift Fleet](aliases-creating.md)\. 
+ **Set up automated build updates\. **Follow the GameTech blog post [ Automating Deployments to Amazon GameLift](https://aws.amazon.com/blogs/gametech/automating-deployments-to-amazon-gamelift/), with sample scripts, to incorporate Amazon GameLift deployments into your build system\.