# Get started with custom servers<a name="gamelift-integration"></a>

This roadmap outlines the key steps to setting up your multiplayer game with custom game servers to use with GameLift\. If you want to use GameLift Realtime Servers, which let you deploy your game client with our ready\-to\-deploy game servers, see [Get started with Realtime Servers](realtime-plan.md)\.

**Tip**  
For more information about GameLift features, including Realtime Servers, [try out the GameLift sample games](gamelift-explore.md)\.

New to GameLift? We recommend that you read [What is Amazon GameLift?](gamelift-intro.md)\. If you're unsure whether GameLift supports your operating systems and development environments, see [Download Amazon GameLift SDKs](gamelift-supported.md) and [Game engines and Amazon GameLift](integration-engines.md)\.

Before you integrate your custom servers with GameLift, you must have an AWS account and you must configure it for GameLift\. For more information, see [Set up an AWS account](setting-up-aws-login.md)\.

**To use custom game servers with GameLift**

1. **Prepare your custom game servers for hosting on GameLift\.**
   + Get the [GameLift Managed Servers SDK](http://aws.amazon.com/gamelift/getting-started/) and build it for your preferred programming language and game engine\. If you're using the Amazon Lumberyard game engine, it already includes a version of the SDK\. For more information, see [GameLift SDKs for custom game servers](gamelift-supported.md#gamelift-supported-servers) and [Game engines and Amazon GameLift](integration-engines.md)\.
   + Add code to your game server project to enable communication with GameLift\. To start and stop game sessions when prompted, and to perform other tasks, a game server must be able to notify GameLift about its status\. For more information, see [Add GameLift to your game server](gamelift-sdk-server-api.md)\.

1. **Prepare your game client to connect to GameLift hosted game sessions\.**
   + Set up a game client to communicate with GameLift through a backend service\. GameLift must communicate with your game client to start game sessions and to place players into games when prompted\.
     + Add the AWS SDK to your backend service and game client project\. For more information, see [GameLift SDKs for client services](gamelift-supported.md#gamelift-supported-clients)\.
     + Add functionality to retrieve information on game sessions, place new game sessions, and reserve space for players on a game session\. For more information, see [Add Amazon GameLift to your game client](gamelift-sdk-client-api.md)\.
     + \(Optional\) Use GameLift FlexMatch for player matchmaking\. For more information, [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.
   + Set up your game client to connect directly to a hosted game session\. Add code to acquire connection information for a game session and, optionally, a reserved player session\. Use this connection information and a unique player ID to communicate with the game server and join the game\. For more information, see [Join a player to a game session](gamelift-sdk-client-api.md#gamelift-sdk-client-api-join)\.

1. **Test your GameLift integration\.**
   + Use Amazon GameLift Local to test your game client and game server integration using a local version of GameLift\. Use GameLift Local to verify that your game components can communicate with GameLift and to test core functionality without uploading game builds or configuring fleets\. For more information, see [Testing your integration](integration-testing-local.md)\.

1. **Build a fleet of computing resources to host your game\.**
   + Package and upload your custom game server build to GameLift\. For more information, see [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md)\.
   + Design a fleet configuration for your game\. Choose the type of computing resources to use, which locations to deploy to, whether to use queues, and other options\. For more information, see [GameLift fleet design guide](fleets-design.md)\.
   + Create fleets and deploy them with your custom game server\. When a fleet is active, it's ready to host game sessions and accept players\. For more information, see [Setting up GameLift fleets](fleets-intro.md)\.
   + Experiment with GameLift fleet configuration settings to optimize usage of your fleet resources\. Adjust the number of game sessions to run concurrently on each instance, or set game session activation limits\. For more information, see [GameLift fleet design guide](fleets-design.md)\.
   + Create a queue to manage how GameLift places new game sessions with available hosting resources\. For more information, see [Design a game session queue](queues-design.md)\.
   + Use automatic scaling to manage your fleet's hosting capacity for expected player demand\. For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\.
   + \(Optional\) Set up a FlexMatch matchmaker with a set of custom matchmaking rules for your game\. For more information, see [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.
**Note**  
After creating a queue, you must update your game client and backend service to use the queue ID when requesting game session placements or matchmaking\.

After you fully integrate GameLift into your game components, monitor and optimize your fleet's availability\. For more information about monitoring your games, see [Viewing your game data in the console](gamelift-console-intro.md)\.

For more information about implementing a game server, see the following topics:

**Topics**
+ [Hosting components overview](gamelift_quickstart_customservers_overview.md)
+ [Prepare your custom game server](gamelift_quickstart_customservers_prepgameserver.md)
+ [Test the integration locally with GameLift Local](gamelift_quickstart_customservers_test.md)
+ [Plan the deployment of your global GameLift resources](gamelift_quickstart_customservers_plan.md)
+ [Deploy your GameLift resources](gamelift_quickstart_customservers_deploy.md)
+ [Design your backend service](gamelift_quickstart_customservers_designbackend.md)
+ [Define and implement metrics and logs solutions](gamelift_quickstart_customservers_metrics.md)
+ [Game launch checklists](gamelift_quickstart_customservers_checklist.md)