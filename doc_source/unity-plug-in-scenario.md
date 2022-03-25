# Deploying a scenario<a name="unity-plug-in-scenario"></a>

The Amazon GameLift Plug\-in for Unity includes the following pre\-built sample scenarios you can customize for your game:
+ **Auth Only** — This scenario creates a game backend service that performs only player authentication and no game server capability\. It creates a Amazon Cognito user pool to store player authentication information and an Amazon API Gateway REST endpoint\-backed AWS Lambda handlers to start a game and view game connection information\. The Lambda handler always returns a 501 Error \(Unimplemented\) 
+ **Single\-Region Fleet** — This scenario creates a game backend service with a single GameLift fleet\. After the player authenticates and starts a game \(with a `POST` request to `/start_game`\), an AWS Lambda handler searches for an existing viable game session with an open player slot on the fleet via `gamelift::SearchGameSession`\. If an open slot is not found, the Lambda creates a new game session via `gamelift::CreateGameSession`\. Once a game start is requested, the game client should poll the backend with `POST` requests to `/get_game_connection` to receive a viable game session\. 
+ **Multi\-Region Fleets with Queue and Custom Matchmaker** — In this scenario, Amazon GameLift queues are used in conjunction with a custom matchmaker\. The custom matchmaker forms matches by grouping up the oldest players in the waiting pool\. Once the placement is done, GameLift publishes a message to the Amazon Simple Notification Service topic in the backend service, triggering a Lambda function to store placement details along with game conection details to a Amazon DynamoDB table\. Subquent `GetGameConnection` calls read from this table and return the connection information to the game client\. 
+ **SPOT Fleets with Queue and Custom Matchmaker** — This scenario is the same as Multi\-Region Fleets with Queue and Custom Matchmaker except it configures three fleets\. Two of the fleets are Spot fleets containing nuanced instance types to provide durability for Spot unavilabilities\. The third fleet is an On\-Demand fleet to serve as a backup in case the other Spot fleets go unavailable\. Using a GameLift queue can keep availability high and cost low\. For more information and best practices about queues, see [Setting up GameLift queues for game session placement](queues-intro.md)\. 
+ **FlexMatch** — On `StartGame` requests, a Lambda creates a matchmaking ticket via `gamelift:StartMatchmaking`\. A separate Lambda listens to FlexMatch Match events similar to the queue example above\. This deployment scenario also uses a low frequency poller to describe incomplete tickets via `gamelift::DescribeMatchmaking`\. The incomplete tickets are periodically described so they are not discarded by GameLift\. This is a best practice recommended by [Track matchmaking events](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-client.html#match-client-track)\. For more information about FlexMatch, see [What is GameLift FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html)\. 

Each sample scenario uses an AWS CloudFormation template to create a stack with all of the resources needed for the sample game\. You can remove the resources by deleting the corresponding AWS CloudFormation stack\.

**Topics**
+ [Updating AWS credentials](#unity-plug-in-configure-creds)
+ [Updating account bootstrap](#unity-plug-in-scenario-boot)
+ [Deploying a sample game scenario](#unity-plug-in-scenario-deploy)
+ [Deleting resources created by the scenario](#unity-plug-in-scenario-delete)

## Updating AWS credentials<a name="unity-plug-in-configure-creds"></a>

The Amazon GameLift Plug\-in for Unity requires security credentials to deploy a scenario\. You can choose to create new credentials or using existing credentials\.

For more information about configuring credentials, see [Understanding and getting your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html)\. 

**To update AWS credentials**

1. In Unity, in the Plug\-in for Unity tab, select the **Deploy** tab\.

1. In the **Deploy** pane, select **AWS Credentials**\.

1. You can create new credentials or choose existing credentials\. 

   To create credentials, select **Create new credentials profile** and then specify the **New Profile Name**, **AWS Access Key ID**, **AWS Secret Key**, and **AWS Region**\.

   To choose existing credentials, select **Choose existing credentials profile** and then select a profile name and **AWS Region**\.

1. In the **Update AWS Credentials** window, select **Update Credentials Profile**\. 

## Updating account bootstrap<a name="unity-plug-in-scenario-boot"></a>

The bootstrap location is an Amazon S3 bucket used during deployment\. It is used to store game server assets and other dependencies\. The AWS Region you select for the bucket must be the same Region you will use for the sample scenario deployment\.

For more information about Amazon S3 buckets, see [Creating, configuring, and working with Amazon Simple Storage Service buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-buckets-s3.html)\.

**To update the account bootstrap location**

1. In Unity, in the Plug\-in for Unity tab, select the **Deploy** tab\.

1. In the **Deploy** pane, select **Update Account Bootstrap**\.

1. In the **Account Bootstrapping** window, you can choose an existing Amazon S3 bucket or create a new Amazon S3 bucket:

   Select **Choose existing Amazon S3 bucket** to choose an existing bucket\. Choose **Update** to save your selection\.

   Select **Create new Amazon S3 bucket** to create a new Amazon Simple Storage Service bucket, then select a **Policy**\. The policy specifies when the Amazon S3 bucket will be expire\. Choose **Create** to create the bucket\. 

## Deploying a sample game scenario<a name="unity-plug-in-scenario-deploy"></a>

You can use a sample scenario to test your game in the cloud\. Each scenario uses a AWS CloudFormation template to create a stack with all of the required resources\. Many of the scenarios require a game server executable and build path\. When the scenario is deployed, some game assets are copied to the bootstrap location as part of deployment\.

You must configure AWS credentials and an AWS account bootstrap to deploy a scenario\.

**To deploy a scenario**

1. In Unity, in the Plug\-in for Unity tab, select the **Deploy** tab\.

1. In the **Deploy** pane, select **Open Deployment UI**\.

1. In the **Deployment** window, select a scenario\. The **Auth Only** scenario does not require a server executable and can deploy quickly\. All other scenarios require a server path and server executable and can take about 30 minutes to deploy\. 

1. Specify a **Game Name**\. It must be unique\. It will be used as part of the AWS CloudFormation stack name when the scenario is deployed\. For example, if you specify **MySampleGame**, the corresponding AWS CloudFormation stack will be named "GameLiftPluginForUnity\-MySampleGame"\. 

1. Select the **Game Server Build Folder Path**\. The build folder path points to the folder containing the server executable and dependencies\. For example, `c:/SampleGame/GameServer`\. 

   You will not be able to select a build folder path if it is not required by the chosen scenario\. 

1. Select the **Game Server Build \.exe File Path**\. The build executable file path points to the game server executable\. For example, `c:/SampleGame/GameServer/SampleGame.exe`\. 

   You will not be able to select a build executable file path if it is not required by the chosen scenario\. 

1. Select **Start Deployment** to initiate deployment of the scenario\. You can follow the status of the update in the **Deployment** window under **Current State**:  
![\[Scenario deployment status update\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_deploy_statex.png)

1. When the scenario completes deployment, **Current State** will be updated to include the **Cognito Client ID** and **API Gateway Endpoint** you can copy and paste into the sample game\.  
![\[Scenario deployment status update\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_deploy_statedone.png)

1. To update sample game settings, on the Unity menu, choose **Go To Client Connection Settings**\. This will display an **Inspector** tab on the right side of the Unity screen\. 

   Make sure **Local Testing Mode** is not selected\.

   Use the API Gateway endpoint value to specify **API Gateway Endpoint** and the Amazon Cognito client ID to specify the **Coginito Client ID**\. Select the same AWS Region you used for the scenario deployment\. You can then rebuild and run the sample game client using the deployed scenario resources\. 

## Deleting resources created by the scenario<a name="unity-plug-in-scenario-delete"></a>

To delete the resources created for the scenario, you must delete the corresponding AWS CloudFormation stack\. 

**To delete resources created by the scenario**

1. In the Amazon GameLift Plug\-in for Unity **Deployment** window, select **View AWS CloudFormation Console** to open the AWS CloudFormation console\. You can also open the AWS CloudFormation console directly with the URL [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\. 

1. In the AWS CloudFormation console, select **Stacks**, and then choose the stack that includes the game name specified during deployment\. For example, if your game name is `MySampleGame`, the AWS CloudFormation stack name will be `GameLiftUnityPlugin-MySampleGame`\. 

1. Select **Delete** to delete the stack\. It may take a few minutes to delete a stack\. When the stack used by the sample scenario is deleted, its status will be `ROLLBACK_COMPLETE`\. 