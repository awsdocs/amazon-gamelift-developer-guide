# Generate player IDs<a name="player-sessions-player-identifiers"></a>

Amazon GameLift uses a player session to represent a player connected to a game session\. GameLift creates a player session each time a player connects to a game session using a game client integrated with GameLift\. When a player leaves a game, the player session ends\. GameLift doesn't reuse player sessions\.

The following code example randomly generates unique player IDs:

```
bool includeBrackets = false;
bool includeDashes = true;
string playerId = AZ::Uuid::CreateRandom().ToString<string>(includeBrackets, includeDashes);
```

For more information about player sessions, see [View data on game and player sessions](gamelift-console-game-player-sessions-metrics.md)\.