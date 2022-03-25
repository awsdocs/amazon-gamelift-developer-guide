# Integrating your game client for Amazon GameLift<a name="gamelift-sdk-client"></a>

The topics in this section describe the managed GameLift functionality that you can add to a game client or game service\. A game client or service may need to handle the following tasks:
+ Request information about active game sessions from the GameLift service\.
+ Join a player to an existing game session\.
+ Create a new game session and join players to it\.
+ Change metadata about an existing game session\.

Adding GameLift to your multiplayer game client is Step 5 in the [Get started with custom servers](gamelift-integration.md)\. The following instructions assume that you've created an AWS account, generated a GameLift\-enabled game server and uploaded it to GameLift, and used GameLift tools \(such as the GameLift console\) to create and configure a virtual fleet to host your game sessions\. When adding GameLift to your game client, you must be able to provide AWS account credentials and specify a fleet to be used with the client\. 

For more information on how game clients interact with the GameLift service and game servers running on GameLift, see [GameLift and game client/server interactions](gamelift-sdk-interactions.md)\. 

**Topics**
+ [Add Amazon GameLift to your game client](gamelift-sdk-client-api.md)
+ [Create game sessions with queues](gamelift-sdk-client-queues.md)
+ [Generate player IDs](player-sessions-player-identifiers.md)