# Managing GameLift hosting resources<a name="resources-intro"></a>

This section provides detailed information about setting up Amazon GameLift managed resources to run your game servers and host game sessions for players\. You must configure and deploy resources, scale capacity to meet player demand, and locate available resources to host game sessions\.

The following diagram illustrates how GameLift resource objects relate to each other\. Use a build or script to create a fleet, give a fleet an alias, and add fleets to a game session queue using their alias\. For games that use FlexMatch matchmaking, use the game session queue and a matchmaking rule set to create a matchmaking configuration\.

![\[The basic structure of GameLift resources and how they relate to each other.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/resources-basicobjects-vsd.png)

****Game server code****  
+ **Build** – Your custom\-built game server software that runs on GameLift and hosts game sessions for your players\. A game build represents the set of files that run your game server on a particular operating system, and that you must integrate with GameLift\. Upload game build files to GameLift in the AWS Regions where you plan to set up fleets\. For more information, see [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md)\.
+ **Script** – Your configuration and custom game logic for use with Realtime Servers\. Configure Realtime Servers for your game clients by creating a script using JavaScript, and add custom game logic to host game sessions for your players\. For more information, see [Upload a Realtime Servers script to GameLift](realtime-script-uploading.md)\.

****Fleet****  
A collection of compute resources that run your game servers and host game sessions for your players\. For information about where you can deploy fleets, see [GameLift hosting in AWS Regions and Local Zones](gamelift-regions.md)\. For information about creating fleets, see [Setting up GameLift fleets](fleets-intro.md)\.

****Alias****  
An abstract identifier for a fleet that you can use to change the fleet that your players are connected to at any time\. For more information, see [Add an alias to a GameLift fleet](aliases-creating.md)\.

****Game session queue****  
A game session placement mechanism that receives requests for new game sessions and searches for available game servers to host the new sessions\. For more information about game session queues, see [Setting up GameLift queues for game session placement](queues-intro.md)\.