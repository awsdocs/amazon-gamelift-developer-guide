# Generate Player IDs<a name="player-sessions-player-identifiers"></a>

Amazon GameLift uses a player session to represent a player connected to a game session\. A player session must be created each time a player connects to a game session\. When a player leaves a game, the player session ends and is not reused\.

Amazon GameLift provides a file called `Lobby.cpp` in the Amazon Lumberyard sample project [ **MultiplayerSample**](https://docs.aws.amazon.com/lumberyard/latest/userguide/sample-project-multiplayer-enhanced.html) that demonstrates how to generate a new, random ID number for every player in every new game session\. You are not required to use the sample code; we provide it as an example\. You can also rewrite the code to persist your own unique, non\-personally identifiable player IDs\.

The following sample code in `Lobby.cpp` randomly generates unique player IDs:

```
bool includeBrackets = false;
bool includeDashes = true;
string playerId = AZ::Uuid::CreateRandom().ToString<string>(includeBrackets, includeDashes);
```

You can view player sessions by Player ID in the [AWS Management Console](https://console.aws.amazon.com/gamelift) for Amazon GameLift\. For more information on player sessions, see [View Data on Game and Player Sessions](gamelift-console-game-player-sessions-metrics.md)\. 