# Get Started with Custom Servers<a name="gamelift-integration"></a>

This roadmap outlines the key steps to setting up your multiplayer games with custom game servers to run with the managed Amazon GameLift solution for custom game servers\. If you're interested in using GameLift Realtime Servers, which let you deploy your game client with our ready\-to\-deploy game servers, see [Get Started with Realtime Servers](realtime-plan.md)\. To learn about other GameLift solutions, see [What Is Amazon GameLift?](gamelift-intro.md)\. 

**Tip**  
[Learn more about ways to explore Amazon GameLift features, including Realtime Servers, using sample games](gamelift-explore.md)\.

New to GameLift? We recommend that you read [What Is Amazon GameLift?](gamelift-intro.md)\. If you're unsure whether GameLift supports your operating systems and development environments, see the topics [GameLift SDKs](gamelift-supported.md) and [Game Engines and Amazon GameLift](integration-engines.md)\.

Before you start integrating with GameLift, you need to have an AWS account and configure it for GameLift\. Learn more at [Set up an AWS account](setting-up-aws-login.md)\. All essential tasks related to creating and managing your game servers can be done using the GameLift console, but you might also want to [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/) 

**To use custom game servers with GameLift**

1. **Prepare your custom game server for hosting on GameLiftAmazon GameLift\.**
   + Get the [Amazon GameLift Server SDK](https://aws.amazon.com/gamelift/getting-started/) and build it for your preferred programming language and game engine\. If you're using the Amazon Lumberyard game engine, a version of the SDK is built in\. See the GameLift SDKs [For custom game servers](gamelift-supported.md#gamelift-supported-servers) and [Game Engines and Amazon GameLift](integration-engines.md)\.
   + Add code to your game server project to enable communication with the GameLift service\. A game server must be able to notify GameLift about its status, to start and stop game sessions when prompted, and to perform other tasks\. See [Add GameLift to your game server](gamelift-sdk-server-api.md)\.

1. **Prepare your game client to connect to GameLift hosted game sessions\.**
   + Set up a game client to communicate with GameLift to start game sessions and place players into games when prompted by a game client\.
     + Add the AWS SDK to your client service project\. See the GameLift SDKs [For client services](gamelift-supported.md#gamelift-supported-clients)\.
     + Add functionality to retrieve information on game sessions, place new game sessions, and \(optionally\) reserve space for players on a game session\. See [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)\. Recommended: Use game session placements to take advantage of FleetIQ and optimize resource usage and player experience\. This option is required if you're using FlexMatch\.
     + \(optional\) Enable the client service to request player matchmaking using FlexMatch\. Learn more in [FlexMatch integration roadmap](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.
   +  Enable your game client to connect directly with a hosted game session\. Add code to acquire connection information for a game session and \(optionally\) a reserved player session\. Use this connection information and a unique player ID to communicate with the game server and join the game\. See [Join a Player to a Game Session](gamelift-sdk-client-api.md#gamelift-sdk-client-api-join)\.

1. **Test your GameLift integration\.**
   + Use Amazon GameLift Local to test your game client and game server integration using a version of the GameLift service that's running locally\. You can use this tool to test your integration without uploading game builds or setting up fleets\. Use GameLift Local to verify that your game components are communicating with the GameLift service and test core functionality\. See [Testing Your Integration](integration-testing-local.md)\. 

1. **Build a fleet of computing resources to host your game\.**
   + Package and upload your custom game server build to the GameLift service\. Be sure to upload your build to each Region where you plan to deploy your game\. See [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md)\.
   + Design a fleet configuration for your game\. Decide, for example, the type of computing resources to use, which Regions to deploy to, whether to use queues, and other options\. See [GameLift fleet design guide](fleets-design.md)\.
   + Create fleets and deploy them with your custom game server\. When a fleet is active, it is ready to host game sessions and accept players\. See [Setting up GameLift fleets](fleets-intro.md)\. 
   + Experiment with GameLift fleet configuration settings and refine as needed to optimize usage of your fleet resources\. Adjust the number of game sessions to run concurrently on each instance, or set game session activation limits\. See [GameLift fleet design guide](fleets-design.md)\. See also how to [Remotely access GameLift fleet instances](fleets-remote-access.md)\.
   + Create a queue to manage how new game sessions are placed with available hosting resources\. See [Design a game session queue](queues-design.md)\. 
   + Enable automatic scaling to manage your fleet's hosting capacity for expected player demand\. See [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\. 
   + \(optional\) Set up a FlexMatch matchmaker with a set of custom matchmaking rules for your game\. Learn more in [FlexMatch integration roadmap](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.
**Note**  
After you create your queues, you must update your client service to use the correct queue ID when requesting game session placements and/or matchmaking\.

After you've fully integrated GameLift into your game components, it's a matter of managing your game server fleets for optimal availability and performance over the long term\. Use GameLift tools to track things like how quickly and efficiently players can find and connect to a game session, overall performance of your game servers over time, and player usage patterns\. See [Viewing Your Game Data in the Console](gamelift-console-intro.md)\.

For more information about implementing a custom game server, see the following topics:

**Topics**
+ [Hosting Components Overview](gamelift_quickstart_customservers_overview.md)
+ [Prepare Your Custom Game Server](gamelift_quickstart_customservers_prepgameserver.md)
+ [Test the Integration Locally with Amazon GameLift Local](gamelift_quickstart_customservers_test.md)
+ [Plan Your Global GameLift Resources Deployment](gamelift_quickstart_customservers_plan.md)
+ [Deploy your Amazon GameLift Resources](gamelift_quickstart_customservers_deploy.md)
+ [Design your Backend Service](gamelift_quickstart_customservers_designbackend.md)
+ [Define and Implement Metrics and Logs Solutions](gamelift_quickstart_customservers_metrics.md)
+ [Prepare for Launch](gamelift_quickstart_customservers_launch.md)
+ [Checklists](gamelift_quickstart_customservers_checklist.md)