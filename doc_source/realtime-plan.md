# Get started with Realtime Servers<a name="realtime-plan"></a>

This roadmap outlines the key steps to getting your multiplayer game clients up and running with the managed GameLift solution with Realtime Servers\. If you have a game with a custom game server, see [Get started with custom servers](gamelift-integration.md)\. 

New to Realtime Servers or unsure about whether this feature is appropriate for your game? We recommend that you read [How Realtime Servers work](realtime-howitworks.md)\. 

**Note**  
If you're familiar with how to integrate and deploy games withGameLift, here's a quick summary of what's different with Realtime Servers:  
Create and upload a Realtime script with optional game logic to run game sessions on Realtime Servers instances\. You no longer need to develop a custom game server and integrate it with the GameLift Server SDK\.
When creating a fleet to host your game sessions, deploy it with the Realtime script instead of a game server build\. 
Integrate your game client with the Realtime Client SDK to manage connections to game sessions\. 

Before you start integration, creaete an AWS account and configure it for GameLift\. Learn more at [Set up an AWS account](setting-up-aws-login.md)\. You can do ll essential tasks related to creating and managing your game servers using the GameLift console, but you may also want to [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/) 

1. **Create a Realtime script for hosting on GameLift\.**
   + Create a Realtime script with your server configuration and optional custom game logic\. Realtime Servers are already built to start and stop game sessions, accept player connections, and manage communication with the GameLift service and between players in a game\. There are also hooks for you to add custom server logic for your game\. Realtime Servers use Node\.js, and the server script is written in JavaScript\. See [Creating a Realtime script](realtime-script.md)\.

1. **Build a fleet of computing resources to host your game\.**
   + Upload the Realtime script to the GameLift service\. Be sure to upload your script to each location where you plan to deploy your game\. See [Upload a Realtime Servers script to GameLift](realtime-script-uploading.md)\.

     Design a fleet configuration for your game\. Choose the type of computing resources to use, which locations to deploy to, whether to use queues, and other options\. See [GameLift fleet design guide](fleets-design.md)\.
   + Create Realtime Servers fleets and deploy them with the Realtime script\. Once a fleet is active, it's ready to host game sessions and accept players\. See [Setting up GameLift fleets](fleets-intro.md)\. 
   + Experiment with GameLift fleet configuration settings and optimize usage of your fleet resources\. Adjust the number of game sessions to run concurrently on each instance, or set game session activation limits\. See [GameLift fleet design guide](fleets-design.md)\. See also how to [Remotely access GameLift fleet instances](fleets-remote-access.md)\.
   + Create a queue to manage how GameLift places new game sessions with available hosting resources\. See [Design a game session queue](queues-design.md)\. 
   + Enable auto\-scaling to manage your fleet's hosting capacity for expected player demand\. See [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\. 
   + \(optional\) Set up a FlexMatch matchmaker with a set of custom matchmaking rules for your game\. Learn more in the [FlexMatch integration roadmap](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.

1. **Prepare your game client to join GameLift\-hosted game sessions\.**
   + Create a mechanism to assign unique player IDs for use with GameLift\.
   + Set up a backend service to send requests to the GameLift for new game sessions and to reserve space for players in existing game sessions\. See [Add Amazon GameLift to your game client](gamelift-sdk-client-api.md)\.
   + \(Optional\) Use FlexMatch for player matchmaking\. Learn more in the [FlexMatch integration roadmap](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.
   + Set up your game client to connect directly to a hosted game session running on a Realtime server and exchange information through messaging\. See [Integrating a game client for Realtime Servers](realtime-client.md)\.

After you've integrated GameLift and Realtime Servers into your game components, monitor and optimize your fleets availability\. For more information about monitoring your games, see [Viewing your game data in the console](gamelift-console-intro.md)\.