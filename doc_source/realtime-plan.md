# Get Started with Realtime Servers<a name="realtime-plan"></a>

This roadmap outlines the key steps to getting your multiplayer game clients up and running with the managed GameLift solution with Realtime Servers\. If you have a game with a custom game server, see [Get Started with Custom Servers](gamelift-integration.md)\. To learn about other GameLift solutions, see [What Is Amazon GameLift?](gamelift-intro.md)\. 

New to Realtime Servers or unsure about whether this feature is appropriate for your game? We recommend that you read [How Realtime Servers Work](realtime-howitworks.md)\. 

**Note**  
If you're familiar with how to integrate and deploy games withGameLift, here's a quick summary of what's different with Realtime Servers:  
Create and upload a Realtime script with optional game logic to run game sessions on Realtime Servers instances\. You no longer need to develop a custom game server and integrate it with the GameLift Server SDK\.
When creating a fleet to host your game sessions, deploy it with the Realtime script instead of a game server build\. 
Integrate your game client with the Realtime Client SDK to manage connections to game sessions\. 

Before you start integration, you need to have an AWS account and configure it for GameLift\. Learn more at [Set up an AWS account](setting-up-aws-login.md)\. All essential tasks related to creating and managing your game servers can be done using the GameLift console, but you may also want to [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/) 

1. **Create a Realtime script for hosting on GameLift\.**
   + Create a Realtime script with your server configuration and optional custom game logic\. Realtime Servers are already built to start and stop game sessions, accept player connections, and manage communication with the GameLift service and between players in a game\. There are also hooks that allows you to add custom server logic for your game\. Realtime Servers is based on Node\.js, and server script is written in JavaScript\. See [Creating a Realtime Script](realtime-script.md)\.

1. **Build a fleet of computing resources to host your game\.**
   + Upload the Realtime script to the GameLift service\. Be sure to upload your script to each region where you plan to deploy your game\. See [Upload a Realtime Servers script to GameLift](realtime-script-uploading.md)\.

     Design a fleet configuration for your game\. Decide, for example, the type of computing resources to use, which regions to deploy to, whether to use queues, and other options\. See [GameLift fleet design guide](fleets-design.md)\.
   + Create Realtime Servers fleets and deploy them with the Realtime script\. Once a fleet is active, it is ready to host game sessions and accept players\. See [Setting up GameLift fleets](fleets-intro.md)\. 
   + Experiment with GameLift fleet configuration settings and refine as needed to optimize usage of your fleet resources\. Adjust the number of game sessions to run concurrently on each instance, or set game session activation limits\. See [GameLift fleet design guide](fleets-design.md)\. See also how to [Remotely access GameLift fleet instances](fleets-remote-access.md)\.
   + Create a queue to manage how new game sessions are placed with available hosting resources\. See [Design a game session queue](queues-design.md)\. 
   + Enable auto\-scaling to manage your fleet's hosting capacity for expected player demand\. See [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\. 
   + \(optional\) Set up a FlexMatch matchmaker with a set of custom matchmaking rules for your game\. Learn more in the [FlexMatch integration roadmap](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.

1. **Prepare your game client to join GameLift\-hosted game sessions\.**
   + Create a mechanism to assign unique player IDs for use with GameLift\.
   + Set up a client service to send requests to the GameLift for new game sessions and to reserve space for players in existing game sessions\. See [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)\.
   + \(optional\) Enable the client service to request player matchmaking using FlexMatch\. Learn more in the [FlexMatch integration roadmap](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.
   +  Enable your game client to connect directly with a hosted game session that is running on a Realtime server and exchange information through messaging\. See [Integrating a Game Client for Realtime Servers](realtime-client.md)\.

Once you've fully integrated GameLift and Realtime Servers into your game components, it's a matter of managing your game server fleets for optimal availability and performance over the long term\. Use GameLift tools to track things like how quickly and efficiently players can find and connect to a game session, overall performance of your game servers over time, and player usage patterns\. See [Viewing Your Game Data in the Console](gamelift-console-intro.md)\.