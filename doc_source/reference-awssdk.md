# Amazon GameLift Service API Reference \(AWS SDK\)<a name="reference-awssdk"></a>

The Amazon GameLift Service API is packaged into the AWS SDK\. [Download the AWS SDK](https://aws.amazon.com/tools/#sdk) or [view the Amazon GameLift API reference documentation](https://docs.aws.amazon.com/gamelift/latest/apireference/)\. 

This page lists the available API actions based on functionality and tasks\. The GameLift service API includes two main categories: \(1\) actions for starting game sessions and getting players into games, and \(2\) actions to manage your GameLift hosting resources\.

## Actions to Manage Games and Players<a name="reference-awssdk-sessions"></a>

Call these API actions from your game client service to start new game sessions, request matchmaking, reserve player slots in active games, and work with game and player session data\.
+ **Start new game sessions for one or more players\.** Use a game session placement to start a new game and place it with the best hosting resources available\. Alternatively, create a new game session on one specific fleet\.
  + [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html) – Request a new game session placement and add one or more players to it\.
  + [DescribeGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionPlacement.html) – Get details on a placement request, including status\.
  + [StopGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopGameSessionPlacement.html) – Cancel a placement request\. 
  + [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html) – Start a new game session on a specific fleet\. *Available in GameLift Local\.* 
+ **Get players into game sessions with FlexMatch matchmaking\.** Group players into a match and start a new game session for them\. Find new players for an existing game using match backfill\.
  + [StartMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchmaking.html) – Request matchmaking for one players or a group who want to play together\. 
  + [DescribeMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html) – Get details on a matchmaking request, including status\.
  + [AcceptMatch](https://docs.aws.amazon.com/gamelift/latest/apireference/API_AcceptMatch.html) – For a match that requires player acceptance, register when a player accepts a proposed match\. 
  + [StopMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html) – Cancel a matchmaking request\. 
  + [StartMatchBackfill](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchBackfill.html) \- Request additional player matches to fill empty slots in an existing game session\.
+ **Get players into existing games\.** Find existing games with available player slots and reserve them for new players\. 
  + [SearchGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_SearchGameSessions.html) – Retrieve all available game sessions or search for game sessions that match a set of criteria\. 
  + [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html) – Reserve an open slot for a player to join a game session\. *Available in GameLift Local\.*
  + [CreatePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSessions.html) – Reserve open slots for multiple players to join a game session\. *Available in GameLift Local\.*
+ **Work with game session and player session data\.** Retrieve current data on existing game sessions and player sessions, and update as needed\.
  + [DescribeGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessions.html) – Retrieve metadata for one or more game sessions, including length of time active and current player count\. *Available in GameLift Local\.*
  + [DescribeGameSessionDetails](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionDetails.html) – Retrieve metadata and the game session protection setting for one or more game sessions\.
  + [GetGameSessionLogUrl](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetGameSessionLogUrl.html) – Get the location of saved logs for a game session\.
  + [DescribePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html) – Get details on player activity, including status, playing time, and player data\. *Available in GameLift Local\.*
  + [UpdateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html) – Change game session settings, such as maximum player count and join policy\.

## Actions to Manage Game Hosting Resources<a name="reference-awssdk-resources"></a>

Use these API actions to configure hosting resources for your game, scale capacity to meet player demand, access performance and utilization metrics, and more\. Most resource management functionality is available in the [GameLift Console](https://console.aws.amazon.com/gamelift/), but you can also make direct calls to the service using the AWS Command Line Interface \(AWS CLI\) tool or the AWS SDK\.
+ **Manage game builds\.** Work with builds of your custom game server, which are uploaded to the GameLift service and deployed on fleets\.
  + [CreateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateBuild.html) – Create a new build using files stored in an Amazon S3 bucket\. To create a build and upload files from a local path, use the AWS CLI\-only command [upload\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html)\.
  + [ListBuilds](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListBuilds.html) – Get a list of all builds uploaded to a GameLift region\.
  + [DescribeBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeBuild.html) – Retrieve information associated with a build\.
  + [UpdateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateBuild.html) – Change build metadata, including build name and version\.
  + [DeleteBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteBuild.html) – Remove a build from GameLift\.
+ **Manage Realtime scripts\.** Work with configuration scripts for use with Realtime Servers\. Unlike custom game builds, scripts can be updated after they are uploaded to the GameLift service\.
  + [CreateScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateScript.html) – Create a new server script to run on Realtime Servers\.
  + [ListScripts](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListScripts.html) – Get a list of all Realtime scripts uploaded to an GameLift region\. 
  + [DescribeScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeScript.html) – Retrieve information associated with a Realtime script\. 
  + [UpdateScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateScript.html) – Change script metadata and upload revised script content\. 
  + [DeleteScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteScript.html) – Remove a Realtime script from GameLift\. 
+ **Manage fleets for game hosting\.** Configure a fleet of hosting resources and deploy your game server or Realtime script to the fleet\.
  + [CreateFleet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html) – Configure and activate a new fleet to run your custom game server build or Realtime script\.
  + [ListFleets](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListFleets.html) – Get a list of all fleets in a GameLift region\.
  + [DeleteFleet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteFleet.html) – Terminate a fleet that is no longer running game servers or hosting players\.
  + View / update fleet configurations\.
    + [DescribeFleetAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetAttributes.html) / [UpdateFleetAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetAttributes.html) – View or change a fleet's metadata and settings for game session protection and resource creation limits\.
    + [DescribeFleetPortSettings](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetPortSettings.html) / [UpdateFleetPortSettings](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetPortSettings.html) – View or change the inbound permissions \(IP address and port setting ranges\) allowed for a fleet\.
    + [DescribeRuntimeConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeRuntimeConfiguration.html) / [UpdateRuntimeConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateRuntimeConfiguration.html) – View or change what server processes \(and how many\) to run on each instance in a fleet\.
+ **Scale fleet capacity\.** Set up fleet automatic scaling or set fleet capacity manually\.
  + [DescribeEC2InstanceLimits](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeEC2InstanceLimits.html) – Retrieve maximum number of instances allowed for the current AWS account and the current usage level\.
  + [DescribeFleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetCapacity.html) – Retrieve the fleets current capacity settings\.
  + [UpdateFleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetCapacity.html) – Manually adjust fleet capacity settings\.
  + Manage auto\-scaling\.
    + [PutScalingPolicy](https://docs.aws.amazon.com/gamelift/latest/apireference/API_PutScalingPolicy.html) – Turn on target\-based auto\-scaling or create a custom auto\-scaling policy, or update an existing policy\.
    + [DescribeScalingPolicies](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeScalingPolicies.html) – Retrieve an existing auto\-scaling policy\.
    + [DeleteScalingPolicy](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteScalingPolicy.html) – Delete an auto\-scaling policy and stop it from affecting a fleet's capacity\.
    + [StartFleetActions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartFleetActions.html) – Restart a fleet's auto\-scaling policies\.
    + [StopFleetActions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopFleetActions.html) – Suspend a fleet's auto\-scaling policies\.
+ **Manage game session queues\.** Set up multi\-fleet, multi\-region Queues to place game sessions with the best available hosting resources\. Queues are required with FlexMatch matchmaking\. 
  + [CreateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSessionQueue.html) – Create a queue for use when processing requests for game session placements\. 
  + [DescribeGameSessionQueues](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionQueues.html) – Retrieve game session queues defined in a GameLift region\.
  + [UpdateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSessionQueue.html) – Change the configuration of a game session queue\.
  + [DeleteGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteGameSessionQueue.html) – Remove a game session queue from the region\.
+ **Manage FlexMatch resources\.** Configure matchmakers for your game and specify match rules to create teams of players to meet your custom specifications\.
  + [CreateMatchmakingConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateMatchmakingConfiguration.html) – Create a matchmaking configuration with instructions for building a player group and placing in a new game session\. 
  + [DescribeMatchmakingConfigurations](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmakingConfigurations.html) – Retrieve matchmaking configurations defined a GameLift region\.
  + [UpdateMatchmakingConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateMatchmakingConfiguration.html) – Change settings for matchmaking configuration\. queue\.
  + [DeleteMatchmakingConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteMatchmakingConfiguration.html) – Remove a matchmaking configuration from the region\.
  + [CreateMatchmakingRuleSet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateMatchmakingRuleSet.html) – Create a set of rules to use when searching for player matches\. 
  + [DescribeMatchmakingRuleSets](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmakingRuleSets.html) – Retrieve matchmaking rule sets defined in a GameLift region\.
  + [ValidateMatchmakingRuleSet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ValidateMatchmakingRuleSet.html) – Verify syntax for a set of matchmaking rules\. 
  + [DeleteMatchmakingRuleSet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteMatchmakingRuleSet.html) – Remove a matchmaking rule set from the region\.
+ **Monitor fleet activity\.** Get up\-to\-the\-minute information on server process and game session activity on a fleet\.
  + [DescribeFleetUtilization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetUtilization.html) – Retrieve statistics on the number of server processes, game sessions, and players that are currently active on a fleet\.
  + [DescribeFleetEvents](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html) – View logged events for a fleet during a specified time span\.
  + [DescribeGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessions.html) – Retrieve game session metadata, including a game's running time and current player count\.
+ **Remotely access an instance\.** Monitor or troubleshoot activity on a specified fleet instance\.
  + [DescribeInstances](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeInstances.html) – Get information on each instance in a fleet, including instance ID, IP address, and status\.
  + [GetInstanceAccess](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetInstanceAccess.html) – Request access credentials needed to remotely connect to a specified instance in a fleet\.
+ **Manage fleet aliases\.** Use aliases to represent your fleets or specify an alternative destination\.
  + [CreateAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateAlias.html) – Define a new alias and optionally assign it to a fleet\.
  + [ListAliases](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListAliases.html) – Get all fleet aliases defined in a GameLift region\.
  + [DescribeAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeAlias.html) – Retrieve information on an existing alias\.
  + [UpdateAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateAlias.html) – Change settings for an alias, such as redirecting it from one fleet to another\.
  + [DeleteAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteAlias.html) – Remove an alias from the region\.
  + [ResolveAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ResolveAlias.html) – Get the fleet ID that a specified alias points to\.
+ **Manage VPC peering connections for fleets\.** Use VPC peering to establish secure access between your GameLift and other AWS resources\.
  + [CreateVpcPeeringAuthorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) – Authorize a peering connection to one of your VPCs\.
  + [DescribeVpcPeeringAuthorizations](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringAuthorizations.html) – Retrieve valid peering connection authorizations\. 
  + [DeleteVpcPeeringAuthorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringAuthorization.html) – Delete a peering connection authorization\.
  + [CreateVpcPeeringConnection](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringConnection.html) – Establish a peering connection between the VPC for a GameLift fleet and one of your VPCs\.
  + [DescribeVpcPeeringConnections](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringConnections.html) – Retrieve information on active or pending VPC peering connections with a GameLift fleet\.
  + [DeleteVpcPeeringConnection](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringConnection.html) – Delete a VPC peering connection with a GameLift fleet\.

## Available Programming Languages<a name="reference-awssdk-langlist"></a>

The AWS SDK with Amazon GameLift is available in the following languages\. See documentation for each language for details on support for development environments\.
+ C\+\+ \([SDK docs](https://aws.amazon.com/sdk-for-cpp/)\) \([Amazon GameLift](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift.html)\)
+ Java \([SDK docs](https://aws.amazon.com/sdk-for-java/)\) \([Amazon GameLift](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/index.html?com/amazonaws/services/gamelift/AmazonGameLift.html)\)
+ \.NET \([SDK docs](https://aws.amazon.com/sdk-for-net/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/GameLift/NGameLift.html)\)
+ Go \([SDK docs](https://aws.amazon.com/sdk-for-go/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-go/api/service/gamelift/)\)
+ Python \([SDK docs](https://aws.amazon.com/sdk-for-python/)\) \([Amazon GameLift](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/gamelift.html)\)
+ Ruby \([SDK docs](https://aws.amazon.com/sdk-for-ruby/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/GameLift.html)\)
+ PHP \([SDK docs](https://aws.amazon.com/sdk-for-php/)\) \([Amazon GameLift](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.GameLift.GameLiftClient.html)\)
+ JavaScript/Node\.js \(\([SDK docs](https://aws.amazon.com/sdk-for-node-js/)\) \([Amazon GameLift](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/GameLift.html)\)