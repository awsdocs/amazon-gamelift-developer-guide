# Integrating your game client for Amazon GameLift<a name="gamelift-sdk-client"></a>

The topics in this section describe the managed GameLift functionality that you can add to a game client or backend service\. A game client or backend service handles the following tasks:
+ Request information about active game sessions from the GameLift service\.
+ Join a player to an existing game session\.
+ Create a new game session and join players to it\.
+ Change metadata about an existing game session\.

Adding GameLift to your multiplayer game client is Step 2 in the [Get started with custom servers](gamelift-integration.md)\. For more information on how game clients interact with the GameLift service and game servers running on GameLift, see [GameLift and game client server interactions](gamelift-sdk-interactions.md)\. 

**Prerequisites**
+ An AWS account\.
+ A game server build uploaded to GameLift\.
+ A fleet for hosting your games\.

**Topics**
+ [Add Amazon GameLift to your game client](gamelift-sdk-client-api.md)
+ [Generate player IDs](player-sessions-player-identifiers.md)