# GameLift service API reference \(AWS SDK\)<a name="reference-awssdk"></a>

This topic provides a task\-based list of API operations for use with GameLift managed hosting solutions, including hosting for custom game servers and Realtime Servers\. These operations are packaged into the AWS SDK in the `aws.gamelift` namespace\. [Download the AWS SDK](https://aws.amazon.com/tools/#sdk) or [view the Amazon GameLift API reference documentation](https://docs.aws.amazon.com/gamelift/latest/apireference/)\.

The API includes two sets of operations for managed game hosting: 
+ [Set up and manage GameLift hosting resources](#reference-awssdk-resources)
+ [Start game sessions and join players](#reference-awssdk-sessions)

The GameLift Service API also contains operations for use with other GameLift tools and solutions\. For a list of FleetIQ APIs, see [FleetIQ API actions](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/reference-awssdk-fleetiq.html)\. For a list of FlexMatch APIs for matchmaking, see [FlexMatch API actions](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/reference-awssdk-flex.html)\. 

## Set up and manage GameLift hosting resources<a name="reference-awssdk-resources"></a>

Call these operations to configure hosting resources for your game servers, scale capacity to meet player demand, access performance and utilization metrics, and more\. These API operations are used with game servers that are hosted on GameLift, including Realtime Servers\. You can use the [GameLift Console](https://console.aws.amazon.com/gamelift/) for most resource management tasks, or you can make calls to the service using the AWS Command Line Interface \(AWS CLI\) tool or the AWS SDK\.

### Prepare game servers for deployment<a name="reference-awssdk-resources-servers"></a>

Upload and configure your game's game server code in preparation for deployment and launching on hosting resources\.

**Manage custom game server builds**
+ [upload\-build](https://docs.aws.amazon.com/cli/latest/reference/gamelift/upload-build.html) – Upload build files from a local path and create a new GameLift build resource\. This operation, available only as an AWS CLI command, is the most common method for uploading game server builds\.
+ [CreateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateBuild.html) – Create a new build using files stored in an Amazon S3 bucket\.
+ [ListBuilds](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListBuilds.html) – Get a list of all builds uploaded to a GameLift region\.
+ [DescribeBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeBuild.html) – Retrieve information associated with a build\.
+ [UpdateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateBuild.html) – Change build metadata, including build name and version\.
+ [DeleteBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteBuild.html) – Remove a build from GameLift\.

**Manage Realtime Servers configuration scripts** 
+ [CreateScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateScript.html) – Upload JavaScript files and create a new GameLift script resource\.
+ [ListScripts](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListScripts.html) – Get a list of all Realtime scripts uploaded to a GameLift region\. 
+ [DescribeScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeScript.html) – Retrieve information associated with a Realtime script\. 
+ [UpdateScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateScript.html) – Change script metadata and upload revised script content\. 
+ [DeleteScript](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteScript.html) – Remove a Realtime script from GameLift\. 

### Set up computing resources for hosting<a name="reference-awssdk-resources-fleets"></a>

Configure hosting resources and deploy them with your game server build or Realtime configuration script\.

**Create and manage fleets**
+ [CreateFleet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html) – Configure and deploy a new GameLift fleet of computing resources to run your game servers\. Once deployed, game servers are automatically launched as configured and ready to host game sessions\.
+ [ListFleets](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListFleets.html) – Get a list of all fleets in a GameLift region\.
+ [DeleteFleet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteFleet.html) – Terminate a fleet that is no longer running game servers or hosting players\.
+ View / update fleet locations\.
  + [CreateFleetLocations](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleetLocations.html) – Add remote locations to an existing fleet that supports multiple locations
  + [DescribeFleetLocationAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetLocationAttributes.html) – Get a list of all remote locations for a fleet and view the current status of each location\.
  + [DeleteFleetLocations](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteFleetLocations.html) – Remove remote locations from a fleet that supports multiple locations\.
+ View / update fleet configurations\.
  + [DescribeFleetAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetAttributes.html) / [UpdateFleetAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetAttributes.html) – View or change a fleet's metadata and settings for game session protection and resource creation limits\.
  + [DescribeFleetPortSettings](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetPortSettings.html) / [UpdateFleetPortSettings](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetPortSettings.html) – View or change the inbound permissions \(IP address and port setting ranges\) allowed for a fleet\.
  + [DescribeRuntimeConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeRuntimeConfiguration.html) / [UpdateRuntimeConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateRuntimeConfiguration.html) – View or change what server processes \(and how many\) to run on each instance in a fleet\.

**Manage fleet capacity**
+ [DescribeEC2InstanceLimits](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeEC2InstanceLimits.html) – Retrieve maximum number of instances allowed for the current AWS account and the current usage level\.
+ [DescribeFleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetCapacity.html) – Retrieve the current capacity settings for a fleet's home Region\.
+ [DescribeFleetLocationCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetLocationCapacity.html) – Retrieve the current capacity settings for each location a multi\-location fleet\.
+ [UpdateFleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetCapacity.html) – Manually adjust capacity settings for a fleet\.
+ Set up auto\-scaling:
  + [PutScalingPolicy](https://docs.aws.amazon.com/gamelift/latest/apireference/API_PutScalingPolicy.html) – Turn on target\-based auto\-scaling or create a custom auto\-scaling policy, or update an existing policy\.
  + [DescribeScalingPolicies](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeScalingPolicies.html) – Retrieve an existing auto\-scaling policy\.
  + [DeleteScalingPolicy](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteScalingPolicy.html) – Delete an auto\-scaling policy and stop it from affecting a fleet's capacity\.
  + [StartFleetActions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartFleetActions.html) – Restart a fleet's auto\-scaling policies\.
  + [StopFleetActions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopFleetActions.html) – Suspend a fleet's auto\-scaling policies\.

**Monitor fleet activity\.**
+ [DescribeFleetUtilization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetUtilization.html) – Retrieve statistics on the number of server processes, game sessions, and players that are currently active on a fleet\.
+ [DescribeFleetLocationUtilization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetLocationUtilization.html) – Retrieve utilization statistics for each location in a multi\-location fleet\.
+ [DescribeFleetEvents](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html) – View logged events for a fleet during a specified time span\.
+ [DescribeGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessions.html) – Retrieve game session metadata, including a game's running time and current player count\.

### Set up queues for optimal game session placement<a name="reference-awssdk-resources-queues"></a>

Set up multi\-fleet, multi\-region queues to place game sessions with the best available hosting resources for cost, latency, and resiliency\.
+ [CreateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSessionQueue.html) – Create a queue for use when processing requests for game session placements\. 
+ [DescribeGameSessionQueues](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionQueues.html) – Retrieve game session queues defined in a GameLift region\.
+ [UpdateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSessionQueue.html) – Change the configuration of a game session queue\.
+ [DeleteGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteGameSessionQueue.html) – Remove a game session queue from the region\.

### Manage aliases<a name="reference-awssdk-resources-aliases"></a>

Use aliases to represent your fleets or create a terminal alternative destination\. Aliases are useful when transitioning game activity from one fleet to another, such as during game server build updates\.
+ [CreateAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateAlias.html) – Define a new alias and optionally assign it to a fleet\.
+ [ListAliases](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ListAliases.html) – Get all fleet aliases defined in a GameLift region\.
+ [DescribeAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeAlias.html) – Retrieve information on an existing alias\.
+ [UpdateAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateAlias.html) – Change settings for an alias, such as redirecting it from one fleet to another\.
+ [DeleteAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteAlias.html) – Remove an alias from the region\.
+ [ResolveAlias](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ResolveAlias.html) – Get the fleet ID that a specified alias points to\.

### Access hosting instances<a name="reference-awssdk-resources-instances"></a>

View information on individual instances in a fleet, or request remote access to a specified fleet instance for troubleshooting\.
+ [DescribeInstances](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeInstances.html) – Get information on each instance in a fleet, including instance ID, IP address, location, and status\.
+ [GetInstanceAccess](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetInstanceAccess.html) – Request access credentials needed to remotely connect to a specified instance in a fleet\.

### Set up VPC peering<a name="reference-awssdk-resources-vpc"></a>

Create and manage VPC peering connections between your GameLift hosting resources and other AWS resources\.
+ [CreateVpcPeeringAuthorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) – Authorize a peering connection to one of your VPCs\.
+ [DescribeVpcPeeringAuthorizations](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringAuthorizations.html) – Retrieve valid peering connection authorizations\. 
+ [DeleteVpcPeeringAuthorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringAuthorization.html) – Delete a peering connection authorization\.
+ [CreateVpcPeeringConnection](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringConnection.html) – Establish a peering connection between the VPC for a GameLift fleet and one of your VPCs\.
+ [DescribeVpcPeeringConnections](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringConnections.html) – Retrieve information on active or pending VPC peering connections with a GameLift fleet\.
+ [DeleteVpcPeeringConnection](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringConnection.html) – Delete a VPC peering connection with a GameLift fleet\.

## Start game sessions and join players<a name="reference-awssdk-sessions"></a>

Call these operations from your game client service to start new game sessions, get information on existing game sessions, and join players to game sessions\. These operations are for use with custom game servers that are hosted on GameLift\. If you're using Realtime Servers, manage game sessions using the [Realtime Servers Client API \(C\#\) Reference](realtime-sdk-csharp-ref.md)\.
+ **Start new game sessions for one or more players\.** 
  + [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html) – Ask GameLift to find the best available hosting resources and start a new game session\. This is the preferred method for creating new game sessions\. It relies on game session queues to track hosting availability across multiple regions, and uses FleetIQ algorithms to prioritize placements based on player latency, hosting cost, location, etc\. 
  + [DescribeGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionPlacement.html) – Get details and status on a placement request\.
  + [StopGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopGameSessionPlacement.html) – Cancel a placement request\. 
  + [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html) – Start a new, empty game session on a specific fleet location\. This operation gives you greater control over where to start the game session, instead of using FleetIQ to evaluate placement options\. You must add players to the new game session in a separate step\. *Available in GameLift Local\.* 
+ **Get players into existing game sessions\.** Find running game sessions with available player slots and reserve them for new players\. 
  + [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html) – Reserve an open slot for a player to join a game session\. *Available in GameLift Local\.*
  + [CreatePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSessions.html) – Reserve open slots for multiple players to join a game session\. *Available in GameLift Local\.*
+ **Work with game session and player session data\.** Manage information on game sessions and player sessions\.
  + [SearchGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_SearchGameSessions.html) – Request a list of active game sessions based on a set of search criteria\. 
  + [DescribeGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessions.html) – Retrieve metadata for specific game sessions, including length of time active and current player count\. *Available in GameLift Local\.*
  + [DescribeGameSessionDetails](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionDetails.html) – Retrieve metadata, including the game session protection setting, for one or more game sessions\.
  + [DescribePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html) – Get details on player activity, including status, playing time, and player data\. *Available in GameLift Local\.*
  + [UpdateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html) – Change game session settings, such as maximum player count and join policy\.
  + [GetGameSessionLogUrl](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetGameSessionLogUrl.html) – Get the location of saved logs for a game session\.

## Available programming languages<a name="reference-awssdk-langlist"></a>

The AWS SDK with Amazon GameLift is available in the following languages\. See documentation for each language for details on support for development environments\.
+ C\+\+ \([SDK docs](https://aws.amazon.com/sdk-for-cpp/)\) \([Amazon GameLift](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift.html)\)
+ Java \([SDK docs](https://aws.amazon.com/sdk-for-java/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-java/latest/reference/index.html?com/amazonaws/services/gamelift/AmazonGameLift.html)\)
+ \.NET \([SDK docs](https://aws.amazon.com/sdk-for-net/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/GameLift/NGameLift.html)\)
+ Go \([SDK docs](https://aws.amazon.com/sdk-for-go/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-go/api/service/gamelift/)\)
+ Python \([SDK docs](https://aws.amazon.com/sdk-for-python/)\) \([Amazon GameLift](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/gamelift.html)\)
+ Ruby \([SDK docs](https://aws.amazon.com/sdk-for-ruby/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/GameLift.html)\)
+ PHP \([SDK docs](https://aws.amazon.com/sdk-for-php/)\) \([Amazon GameLift](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.GameLift.GameLiftClient.html)\)
+ JavaScript/Node\.js \(\([SDK docs](https://aws.amazon.com/sdk-for-node-js/)\) \([Amazon GameLift](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/GameLift.html)\)