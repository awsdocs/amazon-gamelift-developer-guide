# Integrating your Game Client for Amazon GameLift<a name="gamelift-sdk-client"></a>

The topics in this section describe the Amazon GameLift functionality you can add to a game client or game service that handles the following tasks:
+ Requests information about active game sessions from the Amazon GameLift service\.
+ Joins a player to an existing game session\.
+ Creates a new game session\.
+ Changes metadata about an existing game session\.

Adding Amazon GameLift to your multiplayer game client is Step 5 in the [Get Started with Custom Servers](gamelift-integration.md)\. The following instructions assume that you've created an AWS account, generated an Amazon GameLift\-enabled game server and uploaded it to Amazon GameLift, and used Amazon GameLift tools \(such as the Amazon GameLift console\) to create and configure a virtual fleet to host your game sessions\. When adding Amazon GameLift to your game client, you must be able to provide AWS account credentials and specify a fleet to be used with the client\. 

For more information on how game clients interact with the Amazon GameLift service and game servers running on Amazon GameLift, see [Amazon GameLift and Game Client/Server Interactions](gamelift-sdk-interactions.md)\. 

**Topics**
+ [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)
+ [Create Game Sessions with Queues](gamelift-sdk-client-queues.md)
+ [Generate Player IDs](player-sessions-player-identifiers.md)