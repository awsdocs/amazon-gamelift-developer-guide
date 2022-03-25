# Upload a Realtime Servers script to GameLift<a name="realtime-script-uploading"></a>

When you're ready to deploy Realtime Servers for your game, upload completed Realtime server script files to GameLift\. This is done by creating a GameLift script resource and specifying the location of your script files\. You can also update server script files that are already deployed by uploading new files for an existing script resource\.

When you create a new script resource, GameLift assigns a unique script ID \(example: `script-1111aaaa-22bb-33cc-44dd-5555eeee66ff`\) and uploads a copy of the script files\. Upload time depends on the size of your script files and connection speed\.

Once the script resource is created, it can be deployed with a new GameLift Realtime Servers fleet\. GameLift installs your server script onto each instance in the fleet, placing the script files at the following location: `/local/game`\. 

To troubleshoot fleet activation problems that might be related to the server script, see [Debug GameLift fleet issues](fleets-creating-debug.md)\.

## Package script files<a name="realtime-script-uploading-packaging"></a>

Your server script can include one or multiple files combined into a single `.zip` file for uploading\. The zip file must contain all files that your script depends on to run\. 

For uploading, you can store your zipped script files in either a local file directory or in an Amazon S3 bucket\. 

## Upload script files from a local directory<a name="realtime-script-uploading-zip"></a>

If you have your script files stored locally, you can opt to upload them to GameLift from there\. Use either the GameLift console or the AWS CLI tool to create the script resource\. 

------
#### [  GameLift console ]

**To create a script resource**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\. 

1. Use the GameLift main menu to open the **Scripts: Create script** page\. This page contains the script creation form\.

1. In **Script configuration**, enter a script name and version information\. Because the content of a script can be updated, version data can be helpful in tracking the updates\.

1. In **Script code**, choose the **Script type** "Zip file"\. This option lets you specify a zip file that is stored in a local directory\. 

1. Browse for the zip file that contains your script and select it\.

1. After you finish defining the new script record, click **Submit**\. GameLift assigns an ID to the new script and begins uploading the designated zip file\. You can view the new script record, including status, on the console's **Scripts** page\.

------
#### [ AWS CLI ]

To create a script with the AWS CLI, open a command line window and use the `create-script` command to define the new script and upload your server script files\. See complete documentation on this command in the [AWS CLI command reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

**To create a script resource**

1. Place the zip file into a directory where you can make calls to the AWS CLI\. 

1. In a command line window, switch to the directory where the zip file is located\.

1. Enter the `create-script` command and parameters\. For the `-zip-file` parameter, be sure to prepend the string "fileb://" to the name of the zip file\. It identifies the file as binary and ensures that the compressed content will processed correctly\. 

   ```
   AWS gamelift create-script --name [user-defined name of script] --script-version [user-defined version info] --zip-file fileb://[name of zip file] --region [region name]
   ```

   In response to your request, the Amazon GameLift service returns the new script object\. 

   *Examples: *

   ```
   AWS gamelift create-script --name My_Realtime_Server_Script_1 --script-version 1.0.0 --zip-file fileb://myrealtime_script_1.0.0.zip --region us-west-2
   ```

You can view the new script by calling [describe\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-script.html) or by viewing it in the Amazon GameLift console\.

------

## Upload script files in Amazon S3<a name="realtime-script-uploading-s3"></a>

You can opt to store your script files in an Amazon S3 bucket and upload them to GameLift from there\. When you create your script, you specify the S3 bucket location and Amazon GameLift acquires your script files directly from S3\.

**To create a script resource**

1. **Store your script files in an Amazon S3 bucket\. ** Create a \.zip file containing your server script files and upload it to an Amazon S3 bucket in an AWS account that you control\. Take note of the bucket name and the file name \(also called the "key"\); you'll need these when creating a GameLift script\.
**Note**  
GameLift currently does not support uploading from Amazon S3 buckets with names that contain a dot \(\.\)\.

1. **Give GameLift access to your script files\.** Follow the instructions at [Set up a role for GameLift access](setting-up-role.md) to create an IAM role that allows GameLift to access the Amazon S3 bucket containing your server script\. Once you've created the role, take note of the new role's Amazon Resource Name \(ARN\), which you'll need when creating a script\. 

1. **Create a script\.** Use either the Console or the AWS CLI to create a new script record\. To make this request, you must have IAM PassRole permission, as described in [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\. 

------
#### [ GameLift console ]

1. Use the GameLift main menu to open the **Scripts: Create script** page to access the script creation form\.

1. In **Script configuration**, enter a script name and version information\. Because the content of a script can be updated, version data can be helpful in tracking the updates\.

1. In **Script code**, choose the **Script type** "User storage"\. This option lets you specify the S3 bucket containing your script file\. 

1. Enter the storage location information for your S3 bucket:
   + S3 bucket – The bucket name\.
   + S3 key – The name of the file \(zipped file containing your server script\) in the bucket\.
   + S3 role ARN – The ARN value for the IAM role that your created in Step 2\.
   + S3 object version – \(optional\) A specific version number for the S3 file\. This is used only when the S3 bucket has object versioning turned on AND when you want to specify a version other than the latest one\. 

1. Once you finished defining the new script record, click **Submit**\. GameLift assigns an ID to the new script and begins uploading the designated zip file\. You can view the new script record, including status, on the console's **Scripts** page\.

------
#### [ AWS CLI ]

Use the `create-script` command to define the new script and upload your server script files\. See complete documentation on this command in the [AWS CLI command reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

1. Open a command line window and switch to a directory where you can use the AWS CLI tool\. 

1. Enter the `create-script` command with the following parameters: `--name`, `--script-version`, and `--storage-location`\. The storage location parameter specifies the Amazon S3 bucket location of your script files\.

   ```
   AWS gamelift create-script --name [user-defined name of script] --script-version [user-defined version info] --storage-location "Bucket=[S3 bucket name],Key=[name of zip file in S3 bucket],RoleArn=[Access role ARN]" --region [region name]
   ```

   In response to your request, the Amazon GameLift service returns the new script object\. 

   *Examples: *

   ```
   AWS gamelift create-script --name My_Realtime_Server_Script_1 --script-version 1.0.0 --storage-location "Bucket=gamelift-script,Key=myrealtime_script_1.0.0.zip,RoleArn=arn:aws:iam::123456789012:role/S3Access" --region us-west-2
   ```

   You can view the new script by calling [describe\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-script.html) or by viewing it in the Amazon GameLift console\.

------

## Update script files<a name="realtime-script-uploading-update"></a>

You can update the metadata for a script resource using either the GameLift Console or the AWS CLI command [update\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-script.html)\. 

You can also update the script content for a script resource\. Updated script content is deployed to all fleet instances that use the updated script resource\. Once deployed, the updated script is used when starting new game sessions\. It does not affect game sessions that are already running at the time of the update\.

**To update script files: **
+ For script files stored locally: Use either the GameLift Console or the AWS CLI command [update\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-script.html) to upload the updated script `.zip` file\. 
+ For script files stored in an Amazon S3 bucket: Upload the updated script files to the bucket\. Amazon GameLift periodically checks for updated script files and acquires them directly from the S3 bucket\.

You also can change the location where script content is located without having to create a new script record\.