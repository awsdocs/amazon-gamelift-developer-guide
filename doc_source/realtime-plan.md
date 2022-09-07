# Get started with Realtime Servers<a name="realtime-plan"></a>

This roadmap outlines the key steps to get your multiplayer game clients up and running with the managed GameLift solution with Realtime Servers\. For a roadmap for games with a custom game server, see [Get started with custom servers](gamelift-integration.md)\.

If you already know how to integrate and deploy games with GameLift, here's what you can do differently with Realtime Servers:
+ Create and upload a Realtime script with optional game logic to run game sessions on Realtime Servers instances\. You don't need to develop a custom game server and integrate it with the GameLift Managed Servers SDK\.
+ When creating a fleet to host your game sessions, deploy it with the Realtime script instead of a game server build\.
+ Integrate your game client with the Realtime Client SDK to manage connections to game sessions\.

For more information about Realtime Servers and to learn whether this feature is right for your game, see [Integrating games with Amazon GameLift Realtime Servers](realtime-intro.md)\.

Before you start integration, you must have an AWS account configured for GameLift\. For more information, see [Set up an AWS account](setting-up-aws-login.md)\.

1. **Create a Realtime script for hosting on GameLift\.**
   + Create a Realtime script with your server configuration and optional custom game logic\. Realtime Servers are already built to start and stop game sessions, accept player connections, and manage communication with GameLift and between players in a game\. There are also hooks for you to add custom server logic for your game\. Realtime Servers use Node\.js, and the server script is written in JavaScript\. For more information, see [Creating a Realtime script](realtime-script.md)\.

1. **Build a fleet of computing resources to host your game\.**
   + Upload your Realtime script to GameLift\. Be sure to upload the script to each location where you plan to deploy your game\. For more information, see [Upload a Realtime Servers script to GameLift](realtime-script-uploading.md)\.
   + Design a fleet configuration for your game\. Choose the type of computing resources to use, which locations to deploy to, whether to use queues, and other options\. For more information, see [GameLift fleet design guide](fleets-design.md)\.
   + Create Realtime Servers fleets and deploy them with the Realtime script\. When a fleet is active, it's ready to host game sessions and accept players\. For more information, see [Setting up GameLift fleets](fleets-intro.md)\.
   + Experiment with GameLift fleet configuration settings to optimize usage of your fleet resources\. Adjust the number of game sessions to run concurrently on each instance, or set game session activation limits\. For more information, see [GameLift fleet design guide](fleets-design.md) and [Remotely access GameLift fleet instances](fleets-remote-access.md)\.
   + Create a queue to manage how GameLift places new game sessions with available hosting resources\. For more information, see [Design a game session queue](queues-design.md)\.
   + Use auto scaling to manage your fleet's hosting capacity for expected player demand\. For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\.
   + \(Optional\) Set up a GameLift FlexMatch matchmaker with a set of custom matchmaking rules for your game\. For more information, see [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.

1. **Prepare your game client to join GameLift\-hosted game sessions\.**
   + Create a mechanism to assign unique player IDs to players for use with GameLift\.
   + Set up a backend service to send requests to GameLift for new game sessions and to reserve space for players in existing game sessions\. For more information, see [Add Amazon GameLift to your game client](gamelift-sdk-client-api.md)\.
   + \(Optional\) Use FlexMatch for player matchmaking\. For more information, see [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.
   + Set up your game client to connect directly to a hosted game session running on a Realtime server and exchange information through messaging\. For more information, see [Integrating a game client for Realtime Servers](realtime-client.md)\.

After you integrate GameLift and Realtime Servers into your game components, monitor and optimize your fleet's availability\. For more information about monitoring your games, see [Viewing your game data in the console](gamelift-console-intro.md)\.