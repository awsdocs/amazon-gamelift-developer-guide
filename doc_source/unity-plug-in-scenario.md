# Deploying a scenario<a name="unity-plug-in-scenario"></a>

A scenario uses an AWS CloudFormation template to create a stack with the resources needed to customize your game\. This section describes the scenarios GameLift provides and how to use them\. 

**Prerequisites**  
To deploy the scenario, you need an an IAM role for the GameLift service\. For information on how to create a role for GameLift, see [Set up an AWS account](setting-up-aws-login.md)\. 

Each scenario requires permissions to the following resources:
+ GameLift
+ Amazon S3
+ AWS CloudFormation
+ API Gateway
+ AWS Lambda
+ AWS WAFV2
+ Amazon Cognito

## Scenarios<a name="unity-plug-in-scenario-examples"></a>

The Amazon GameLift Plug\-in for Unity includes the following scenarios: 

**Auth only**  
This scenario creates a game backend service that performs player authentication without game server capability\. The template creates the following resources in your account:
+ An Amazon Cognito user pool to store player authentication information\.
+ An Amazon API Gateway REST endpoint\-backed AWS Lambda handler that starts games and views game connection information\.

**Single\-Region fleet**  
This scenario creates a game backend service with a single GameLift fleet\. It creates the following resources: 
+ An Amazon Cognito user pool for a player to authenticate and start a game\. 
+ An AWS Lambda handler to search for an existing game session with an open player slot on the fleet\. If it can't find a open slot, it creates a new game session\. 

**Multi\-Region fleet with a queue and custom matchmaker**  
This scenario forms matches by using GameLift queues and a custom matchmaker to group together the oldest players in the waiting pool\. It creates the following resources:
+ An Amazon Simple Notification Service topic that GameLift publishes messages to\. For more information on SNS topics and notifications, see [Set up event notification for game session placement](queue-notification.md)\. 
+ A Lambda function that's invoked by the message that communicates placement and game connection details\.
+ An Amazon DynamoDB table to store placement and game connection details\. `GetGameConnection` calls read from this table and return the connection information to the game client\. 

**Spot fleets with a queue and custom matchmaker**  
This scenario forms matches by using GameLift queues and a custom matchmaker and configures three fleets\. It creates the following resources:
+ Two Spot fleets that contain different instance types to provide durability for Spot unavailability\.
+ An On\-Demand fleet that acts as a backup for the other Spot fleets\. For more information on designing your fleets, see [GameLift fleet design guide](fleets-design.md)\.
+ A GameLift queue to keep server availability high and cost low\. For more information and best practices about queues, see [Design a game session queue](queues-design.md)\. 

**FlexMatch**  
This scenario uses FlexMatch, a managed matchmaking service, to match game players together\. For more information about FlexMatch, see [What is GameLift FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html)\. This scenario creates the following resources:
+ A Lambda function to create a matchmaking ticket after it receives `StartGame` requests\. 
+ A separate Lambda function to listen to FlexMatch match events\.

To avoid unnecessary charges on your AWS account, remove the resources created by each scenario after you are done using them\. Delete the corresponding AWS CloudFormation stack\. 

## Updating AWS credentials<a name="unity-plug-in-configure-creds"></a>

The Amazon GameLift Plug\-in for Unity requires security credentials to deploy a scenario\. You can either create new credentials or use existing credentials\.

For more information about configuring credentials, see [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\. 

**To update AWS credentials**

1. In Unity, in the Plug\-in for Unity tab, choose the **Deploy** tab\.

1. In the **Deploy** pane, choose **AWS Credentials**\.

1. You can create new AWS credentials or choose existing credentials\. 
   + To create credentials, choose **Create new credentials profile**, and then specify the **New Profile Name**, **AWS Access Key ID**, **AWS Secret Key**, and **AWS Region**\.
   + To choose existing credentials, choose **Choose existing credentials profile** and then select a profile name and **AWS Region**\.

1. In the **Update AWS Credentials** window, choose **Update Credentials Profile**\. 

## Updating account bootstrap<a name="unity-plug-in-scenario-boot"></a>

The bootstrap location is an Amazon S3 bucket used during deployment\. It's used to store game server assets and other dependencies\. The AWS Region you choose for the bucket must be the same Region you will use for the scenario deployment\.

For more information about Amazon S3 buckets, see [Creating, configuring, and working with Amazon Simple Storage Service buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-buckets-s3.html)\.

**To update the account bootstrap location**

1. In Unity, in the Plug\-in for Unity tab, choose the **Deploy** tab\.

1. In the **Deploy** pane, choose **Update Account Bootstrap**\.

1. In the **Account Bootstrapping** window, you choose an existing Amazon S3 bucket or create a new Amazon S3 bucket:
   + To choose an existing bucket, choose **Choose existing Amazon S3 bucket** and **Update** to save your selection\.
   + Choose **Create new Amazon S3 bucket** to create a new Amazon Simple Storage Service bucket, then choose a **Policy**\. The policy specifies when the Amazon S3 bucket will be expire\. Choose **Create** to create the bucket\. 

## Deploying a game scenario<a name="unity-plug-in-scenario-deploy"></a>

You can use a scenario to test your game with GameLift\. Each scenario uses a AWS CloudFormation template to create a stack with the required resources\. Most of the scenarios require a game server executable and build path\. When you deploy the scenario, GameLift copies game assets to the bootstrap location as part of deployment\.

You must configure AWS credentials and an AWS account bootstrap to deploy a scenario\.

**To deploy a scenario**

1. In Unity, in the Plug\-in for Unity tab, choose the **Deploy** tab\.

1. In the **Deploy** pane, choose **Open Deployment UI**\.

1. In the **Deployment** window, choose a scenario\.

1. Enter a **Game Name**\. It must be unique\. The game name is part of the AWS CloudFormation stack name when you deploy the scenario\. 

1. Choose the **Game Server Build Folder Path**\. The build folder path points to the folder containing the server executable and dependencies\.

1. Choose the **Game Server Build \.exe File Path**\. The build executable file path points to the game server executable\.

1. Choose **Start Deployment** to begin deploying a scenario\. You can follow the status of the update in the **Deployment** window under **Current State**\.Scenarios can take up to 30 minutes to deploy\.  
![\[Scenario deployment status update\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_deploy_statex.png)

1. When the scenario completes deployment, the **Current State** updates to include the **Cognito Client ID** and **API Gateway Endpoint** that you can copy and paste into the game\.  
![\[Scenario deployment status update\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_deploy_statedone.png)

1. To update game settings, on the Unity menu, choose **Go To Client Connection Settings**\. This displays an **Inspector** tab on the right side of the Unity screen\.

1. Deselect **Local Testing Mode**\.

1. Enter the **API Gateway Endpoint** and the **Coginito Client ID**\. Choose the same AWS Region you used for the scenario deployment\. You can then rebuild and run the game client using the deployed scenario resources\. 

## Deleting resources created by the scenario<a name="unity-plug-in-scenario-delete"></a>

To delete the resources created for the scenario, delete the corresponding AWS CloudFormation stack\. 

**To delete resources created by the scenario**

1. In the Amazon GameLift Plug\-in for Unity **Deployment** window, select **View AWS CloudFormation Console** to open the AWS CloudFormation console\. 

1. In the AWS CloudFormation console, choose **Stacks**, and then choose the stack that includes the game name specified during deployment\.

1. Choose **Delete** to delete the stack\. It may take a few minutes to delete a stack\. After AWS CloudFormation deletes the stack used by the scenario, its status changes to `ROLLBACK_COMPLETE`\. 