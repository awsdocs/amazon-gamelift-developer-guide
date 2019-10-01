# Get Started with Custom Servers<a name="gamelift-integration"></a>

This roadmap outlines the key steps to getting your multiplayer games with custom game servers up and running on GameLift\. If you're interested in using GameLift Realtime Servers, which lets you deploy your game client with our ready\-to\-deploy game servers, see [Get Started with Realtime Servers](realtime-plan.md)\.

**Tip**  
[Learn more about ways to explore Amazon GameLift features, including Realtime Servers, using sample games](gamelift-explore.md)\.

New to GameLift? We recommend that you read [What Is Amazon GameLift?](gamelift-intro.md) If you're unsure whether GameLift supports your operating systems and development environments, see the topics [Amazon GameLift SDKs](gamelift-supported.md) and [Game Engines and Amazon GameLift](integration-engines.md)\.

Before you start integration, you need to have an AWS account and configure it for Amazon GameLift\. Learn more at [Set Up an AWS Account](setting-up-aws-login.md)\. All essential tasks related to creating and managing your game servers can be done using the Amazon GameLift console, but you may also want to [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/) 

1. **Prepare your custom game server for hosting on Amazon GameLift\.**
   + Get the [Amazon GameLift Server SDK](https://aws.amazon.com/gamelift/getting-started/) and build it for your preferred programming language and game engine\. If you're using the Amazon Lumberyard game engine, a version of the SDK is built in\. See the Amazon GameLift SDKs [For Custom Game Servers](gamelift-supported.md#gamelift-supported-servers) and [Game Engines and Amazon GameLift](integration-engines.md)\.
   + Add code to your game server project to enable communication with the Amazon GameLift service\. A game server must be able to notify Amazon GameLift about its status, start/stop game sessions when prompted, and other tasks\. See [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)\.

1. **Prepare your game client to connect to Amazon GameLift\-hosted game sessions\.**
   + Set up a client service to communicate with Amazon GameLift service in order to start game sessions and place players into games when prompted by a game client\.
     + Add the AWS SDK to your client service project\. See the Amazon GameLift SDKs [For Client Services](gamelift-supported.md#gamelift-supported-clients)\.
     + Add functionality to retrieve information on game sessions, place new game sessions and \(optionally\) reserve space for players on a game session\. See [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)\. Recommended: Use game session placements to take advantage of FleetIQ and optimize resource usage and player experience\. This option is required if you're using FlexMatch\.
     + \(optional\) Enable the client service to request player matchmaking using FlexMatch\. Learn more in [FlexMatch Integration Roadmap](match-tasks.md)\.
   +  Enable your game client to connect directly with a hosted game session\. Add code to acquire connection information for a game session and \(optionally\) a reserved player session\. Use this connection information and a unique player ID to communicate with the game server and join the game\. See [Join a Player to a Game Session](gamelift-sdk-client-api.md#gamelift-sdk-client-api-join)\.

1. **Test your Amazon GameLift integration\.**
   + Use Amazon GameLift Local to test your game client and game server integration using a version of the Amazon GameLift service running locally\. You can use this tool to test your integration without having to upload game builds and set up fleets\. You can verify that your game components are communicating with the Amazon GameLift service, and test core functionality\. See [Testing Your Integration](integration-testing-local.md)\. 

1. **Build a fleet of computing resources to host your game\.**
   + Package and upload your custom game server build to the Amazon GameLift service\. Be sure to upload your build to each region where you plan to deploy your game\. See [Upload a Custom Game Server Build to Amazon GameLift](gamelift-build-cli-uploading.md)\.
   + Design a fleet configuration for your game\. Decide, for example, the type of computing resources to use, which regions to deploy to, whether to use queues, and other options\. See [Design a Amazon GameLift Fleet for Your Game](fleets-design.md)\.
   + Create fleets and deploy them with your custom game server\. Once a fleet is active, it is ready to host game sessions and accept players\. See [Setting Up Amazon GameLift Fleets](fleets-intro.md)\. 
   + Experiment with Amazon GameLift fleet configuration settings and refine as needed to optimize usage of your fleet resources\. Adjust the number of game sessions to run concurrently on each instance, or set game session activation limits\. See [Design a Amazon GameLift Fleet for Your Game](fleets-design.md)\. See also how to [Remotely Access Fleet Instances](fleets-remote-access.md)\.
   + Create a queue to manage how new game sessions are placed with available hosting resources\. See [Design a Game Session Queue](queues-design.md)\. 
   + Enable auto\-scaling to manage your fleet's hosting capacity for expected player demand\. See [Scaling Amazon GameLift Fleet Capacity](fleets-manage-capacity.md)\. 
   + \(optional\) Set up a FlexMatch matchmaker with a set of custom matchmaking rules for your game\. Learn more in [FlexMatch Integration Roadmap](match-tasks.md)\.
**Note**  
Once you've created your queues, you'll need to update your client service to use the correct queue ID when requestion game session placements and/or matchmaking\.

Once you've fully integrated Amazon GameLift into your game components, it's a matter of managing your game server fleets for optimal availability and performance over the long term\. Use Amazon GameLift tools to track things like how quickly and efficiently players can find and connect to a game session, overall performance of your game servers over time, and player usage patterns\. See [Viewing Your Game Data in the Console](gamelift-console-intro.md)\.