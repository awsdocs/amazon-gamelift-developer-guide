# Upload a Realtime Servers script to GameLift<a name="realtime-script-uploading"></a>

When you're ready to deploy Realtime Servers for your game, upload completed Realtime server script files to GameLift\. Do this by creating a GameLift script resource and specifying the location of your script files\. You can also update server script files that are already deployed by uploading new files for an existing script resource\.

When you create a new script resource, GameLift assigns it a unique script ID \(for example, `script-1111aaaa-22bb-33cc-44dd-5555eeee66ff`\) and uploads a copy of the script files\. Upload time depends on the size of your script files and on your connection speed\.

After you create the script resource, GameLift deploys the script with a new Realtime Servers fleet\. GameLift installs your server script onto each instance in the fleet, placing the script files in `/local/game`\.

To troubleshoot fleet activation problems related to the server script, see [Debug GameLift fleet issues](fleets-creating-debug.md)\.

## Package script files<a name="realtime-script-uploading-packaging"></a>

Your server script can include one or more files combined into a single \.zip file for uploading\. The \.zip file must contain all files that your script needs to run\.

You can store your zipped script files in either a local file directory or in an Amazon Simple Storage Service \(Amazon S3\) bucket\.

## Upload script files from a local directory<a name="realtime-script-uploading-zip"></a>

If you have your script files stored locally, you can upload them to GameLift from there\. To create the script resource, use either the GameLift console or the [AWS Command Line Interface \(AWS CLI\)](http://aws.amazon.com/cli/)\.

------
#### [  GameLift console ]

**To create a script resource**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. In the navigation pane, choose **Hosting**, **Scripts**\.

1. On the **Scripts** page, choose **Create script**\.

1. On the **Create script** page, under **Script settings,** do the following:

   1. For **Name**, enter a script name\.

   1. \(Optional\) For **Version**, enter version information\. Because you can update the content of a script, version data can be helpful in tracking updates\.

   1. For **Script source**, choose **Upload a \.zip file**\.

   1. For **Script files**, choose **Choose file**, browse for the \.zip file that contains your script, and then choose that file\.

1. \(Optional\) Under **Tags**, add tags to the script by entering **Key** and **Value** pairs\.

1. Choose **Create**\.

   GameLift assigns an ID to the new script and uploads the designated \.zip file\. You can view the new script, including its status, on the **Scripts** page\.

------
#### [ AWS CLI ]

Use the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html) AWS CLI command to define the new script and upload your server script files\.

**To create a script resource**

1. Place the \.zip file into a directory where you can use the AWS CLI\.

1. Open a command line window and switch to the directory where you placed the \.zip file\.

1. Enter the following create\-script command and parameters\. For the \-\-zip\-file parameter, be sure to add the string `fileb://` to the name of the \.zip file\. It identifies the file as binary so that GameLift processes the compressed content\.

   ```
   aws gamelift create-script \
       --name user-defined name of script \
       --script-version user-defined version info \
       --zip-file fileb://name of zip file \
       --region region name
   ```

   *Example*

   ```
   aws gamelift create-script \
       --name "My_Realtime_Server_Script_1" \
       --script-version "1.0.0" \
       --zip-file fileb://myrealtime_script_1.0.0.zip \
       --region us-west-2
   ```

   In response to your request, GameLift returns the new script object\.

1. To view the new script, call `[describe\-script](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-script.html)`\.

------

## Upload script files from Amazon S3<a name="realtime-script-uploading-s3"></a>

You can store your script files in an Amazon S3 bucket and upload them to GameLift from there\. When you create your script, you specify the S3 bucket location and GameLift retrieves your script files from Amazon S3\.

**To create a script resource**

1. **Store your script files in an S3 bucket\.** Create a \.zip file containing your server script files and upload it to an S3 bucket in an AWS account that you control\. Take note of the object URI—you need this when creating a GameLift script\.
**Note**  
GameLift doesn't support uploading from S3 buckets with names that contain a period \(\.\)\.

1. **Give GameLift access to your script files\.** To create an AWS Identity and Access Management \(IAM\) role that allows GameLift to access the S3 bucket containing your server script, follow the instructions in [Set up an IAM service role for GameLift](setting-up-role.md)\. After you create the new role, take note of its name, which you need when creating a script\.

1. **Create a script\.** Use the GameLift console or the AWS CLI to create a new script record\. To make this request, you must have the IAM `PassRole` permission, as described in [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\.

------
#### [ GameLift console ]

1. In the [GameLift console](https://console.aws.amazon.com/gamelift), in the navigation pane, choose **Hosting**, **Scripts**\.

1. On the **Scripts** page, choose **Create script**\.

1. On the **Create script** page, under **Script settings**, do the following:

   1. For **Name**, enter a script name\.

   1. \(Optional\) For **Version**, enter version information\. Because you can update the content of a script, version data can be helpful in tracking updates\.

   1. For **Script source**, choose **Amazon S3 URI**\.

   1. Enter the **S3 URI** of the script object that you uploaded to Amazon S3, and then choose the **Object version**\. If you don't remember the Amazon S3 URI and object version, choose **Browse S3**, and then search for the script object\.

1. \(Optional\) Under **Tags**, add tags to the script by entering **Key** and **Value** pairs\.

1. Choose **Create**\.

   GameLift assigns an ID to the new script and uploads the designated \.zip file\. You can view the new script, including its status, on the **Scripts** page\.

------
#### [ AWS CLI ]

Use the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-script.html) AWS CLI command to define the new script and upload your server script files\.

1. Open a command line window and switch to a directory where you can use the AWS CLI\.

1. Enter the following create\-script command and parameters\. The \-\-storage\-location parameter specifies the Amazon S3 bucket location of your script files\.

   ```
   aws gamelift create-script \
       --name [user-defined name of script] \
       --script-version [user-defined version info] \
       --storage-location "Bucket"=S3 bucket name,"Key"=name of zip file in S3 bucket,"RoleArn"=Access role ARN \
       --region region name
   ```

   *Example*

   ```
   aws gamelift create-script \
       --name "My_Realtime_Server_Script_1" \
       --script-version "1.0.0" \
       --storage-location "Bucket"="gamelift-script","Key"="myrealtime_script_1.0.0.zip","RoleArn"="arn:aws:iam::123456789012:role/S3Access" \
       --region us-west-2
   ```

   In response to your request, GameLift returns the new script object\.

1. To view the new script, call [https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-script.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-script.html)\.

------

## Update script files<a name="realtime-script-uploading-update"></a>

You can update the metadata for a script resource using either the GameLift console or the [https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-script.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-script.html) AWS CLI command\.

You can also update the script content for a script resource\. GameLift deploys script content to all fleet instances that use the updated script resource\. When the updated script is deployed, instances use it when starting new game sessions\. Game sessions that are already running at the time of the update don't use the updated script\.

**To update script files**
+ For script files stored locally, to upload the updated script \.zip file, use either the GameLift console or the update\-script command\.
+ For script files stored in an Amazon S3 bucket, upload the updated script files to the S3 bucket\. GameLift periodically checks for updated script files and retrieves them directly from the S3 bucket\.