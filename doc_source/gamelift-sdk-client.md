# Integrate your game client with GameLift<a name="gamelift-sdk-client"></a>

The topics in this section describe the managed Amazon GameLift functionality that you can add to a backend service\. A backend service handles the following tasks:
+ Requests information about active game sessions from GameLift\.
+ Joins a player to an existing game session\.
+ Creates a new game session and joins players to it\.
+ Changes metadata for an existing game session\.

For more information about how game clients interact with GameLift and game servers running on GameLift, see [GameLift and game client server interactions](gamelift-sdk-interactions.md)\. 

**Prerequisites**
+ An AWS account\.
+ A game server build uploaded to GameLift\.
+ A fleet for hosting your games\.

**Topics**
+ [Add GameLift to your game client](gamelift-sdk-client-api.md)
+ [Generate player IDs](player-sessions-player-identifiers.md)