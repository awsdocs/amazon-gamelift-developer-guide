# Upload a Realtime Servers Script to Amazon GameLift<a name="realtime-script-uploading"></a>

When you have completed a Realtime server script for your game, upload the script files to Amazon GameLift for deployment with your Realtime Servers\. This is done by creating an Amazon GameLift script record and providing the script files\. You have two options when providing the script files:
+ Create a script with a zip file that is stored on a local directory\.
+ Create a script with a zip file that is stored in an Amazon S3 bucket\.

When you create a new script , Amazon GameLift assigns a unique script ID \(example: `script-1111aaaa-22bb-33cc-44dd-5555eeee66ff`\) and adds the creation time stamp, uploaded file size, and other metadata\. Upload time depends on the size of your script files and the connection speed\.

At this point, the script can be deployed with a new Amazon GameLift Realtime Servers fleet\. Amazon GameLift installs your server script onto each instance in the fleet, placing the script files at the following location: `/local/game`\. 

To troubleshoot fleet activation problems that might be related to the server script, see [Debug Fleet Issues](fleets-creating-debug.md)\.

## Package Your Script Files<a name="realtime-script-uploading-packaging"></a>

Your server script can include one or multiple files\. For uploading, combine all files that your JavaScript depends on to run into a single zip file\. 

## Upload Local Script Files<a name="realtime-script-uploading-zip"></a>

You can use either the Amazon GameLift console or the AWS CLI tool to create a script\. 

------
#### [  GameLift Console ]

**To create a script**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\. 

1. Use the GameLift main menu to open the **Scripts: Create script** page to access the script creation form\.

1. In **Script configuration**, enter a script name and version information\. Because the content of a script can be updated, version data can be helpful in tracking the updates\.

1. In **Script code**, choose the **Script type** "Zip file"\. This option lets you specify a zip file that is stored in a local directory\. 

1. Browse for the zip file that contains your script and select it\.

1. Once you finished defining the new script record, click **Submit**\. Amazon GameLift assigns an ID to the new script and begins uploading the designated zip file\. You can view the new script record, including status, on the console's **Scripts** page\.

------
#### [ AWS CLI ]

To create a script with the AWS CLI, open a command line window and use the `create-script` command to define the new script and upload your server script files\. See complete documentation on this command in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

**To create a script**

1. Place the zip file into a directory that you can make calls to the AWS CLI\. 

1. In a command line window, switch to the directory where the zip file is located\.

1. Enter the `create-script` command and parameters\. For the `-zip-file` parameter, be sure to prepend the string "fileb://" to the name of the zip file\. It identifies the file as binary and ensures that the compressed content will processed correctly\. 

   ```
   aws gamelift create-script --name [user-defined name of script] --version [user-defined version info] --zip-file fileb://[name of zip file] --region [region name]
   ```

   In response to your request, the Amazon GameLift service returns the new script object\. 

   *Examples: *

   ```
   aws gamelift create-script --name My_Realtime_Server_Script_1 --version 1.0.0 --zip-file fileb://myrealtime_script_1.0.0.zip --region us-west-2
   ```

You can view the new script by calling [describe\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-script.html) or by viewing it in the Amazon GameLift console\.

------

## Upload Script Files in Amazon S3<a name="realtime-script-uploading-s3"></a>

You can opt to store your script files in an Amazon S3 bucket and upload them to Amazon GameLift from there\. When you create your script, you specify the S3 bucket location and Amazon GameLift acquires your script files directly from S3\.

**To create a script**

1. **Store your script files in an Amazon S3 bucket\. ** Upload the zip file containing your server script into an Amazon S3 bucket in an AWS account that you control\. Take note of the bucket name and the file name \(also called the "key"\); you'll need these when creating an Amazon GameLift script\.

1. **Give Amazon GameLift access to your script files\.** Follow the instructions at [Set Up a Role for Amazon GameLift Access](setting-up-role.md) to create an IAM role that allows Amazon GameLift to access the Amazon S3 bucket containing your server script\. Once you've created the role, take note of the new role's Amazon Resource Name \(ARN\), which you'll need when creating a script\. 

1. **Create a script\.** Use either the Console or the AWS CLI to create a new script record\.

------
#### [ GameLift Console ]

1. Use the GameLift main menu to open the **Scripts: Create script** page to access the script creation form\.

1. In **Script configuration**, enter a script name and version information\. Because the content of a script can be updated, version data can be helpful in tracking the updates\.

1. In **Script code**, choose the **Script type** "User storage"\. This option lets you specify the S3 bucket containing your script file\. 

1. Enter the storage location information for your S3 bucket:
   + S3 bucket – The bucket name\.
   + S3 key – The name of the file \(zipped file containing your server script\) in the bucket\.
   + S3 role ARN – The ARN value for the IAM role that your created in Step 2\.
   + S3 object version – \(optional\) A specific version number for the S3 file\. This is used only when the S3 bucket has object versioning turned on AND when you want to specify a version other than the latest one\. 

1. Once you finished defining the new script record, click **Submit**\. Amazon GameLift assigns an ID to the new script and begins uploading the designated zip file\. You can view the new script record, including status, on the console's **Scripts** page\.

------
#### [ AWS CLI ]

Use the `create-script` command to define the new script and upload your server script files\. See complete documentation on this command in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

1. Open a command line window and switch to a directory where you can use the AWS CLI tool\. 

1. Enter the `create-script` command with the following parameters: `--name`, `--version`, and `--storage-location`\. The storage location parameter specifies the Amazon S3 bucket location of your script files\.

   ```
   aws gamelift create-script --name [user-defined name of script] --version [user-defined version info] --storage-location "Bucket=[S3 bucket name],Key=[name of zip file in S3 bucket],RoleArn=[Access role ARN]" --region [region name]
   ```

   In response to your request, the Amazon GameLift service returns the new script object\. 

   *Examples: *

   ```
   aws gamelift create-script --name My_Realtime_Server_Script_1 --version 1.0.0 --storage-location "Bucket=gamelift-script,Key=myrealtime_script_1.0.0.zip,RoleArn=arn:aws:iam::123456789012:role/S3Access" --region us-west-2
   ```

   You can view the new script by calling [describe\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-script.html) or by viewing it in the Amazon GameLift console\.

------

## Update Script Files<a name="realtime-script-uploading-update"></a>

You can update either the metadata or the script content at any time using either the GameLift Console or the AWS CLI command [update\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-script.html)\. When you update script content, the new script is used for all new game sessions, but does not affect currently running game sessions that were created before the script update\. 

If the script is stored in an Amazon S3 bucket under your account, you can update the script content by uploading a new zip file to the bucket\. If the script is provided as a local zip file, you must upload the new file to GameLift\. 

You also can change the location where script content is located without having to create a new script record\.