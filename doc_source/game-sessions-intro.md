# How Players Connect to Games<a name="game-sessions-intro"></a>

A *game session* is an instance of your game running on Amazon GameLift\. To play your game, players can either find and join an existing game session or create a new game session and join it\. Players join by creating a *player session* for the game session\. If the game session is open for players—that is, it is accepting new players and has an open player slot—Amazon GameLift reserves a slot for the player and provides connection information back to the player\. The player can then connect to the game session and claim the reserved slot\.

For detailed information on creating and managing games sessions and player sessions, see [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)\. 

## Game and Player Session Features<a name="game-sessions-intro-features"></a>

Amazon GameLift provides several features related to game and player sessions:

### Host game sessions on best available resources across multiple regions<a name="game-sessions-intro-features-best-resources"></a>

Choose from multiple options when configuring how Amazon GameLift selects resources to host new game sessions\. If you're running multiple fleets in more than one region, you can set up **game session queues **that can place a new game session on any fleet regardless of region\. This feature can significantly improve the Amazon GameLift service's ability to efficiently balance resource usage and respond to changes in player demand, decreased capacity, outage events, and other issues\. As a result, using queues can decrease the manual overhead needed to monitor and balance resources\. You can manage queues and track queue performance metrics in the Amazon GameLift Console\.

With the queues feature, you have the option of placing game sessions based on player latency information\. This feature is particularly effective when supporting a matchmaking service\. Requests for a new game session can also request new player sessions for one or more players\. If you include latency data for each player by region, Amazon GameLift can choose a fleet in a region that provides the best possible experience for all the players\.

### Control player access to game sessions<a name="game-sessions-intro-features-player-access"></a>

Set a game session to allow or deny join requests from new players, regardless of the number of players currently connected\. You might use this feature to enable private sessions, to limit access for troubleshooting or other problem resolution, etc\.

### Add custom game and player data<a name="game-sessions-intro-features-custom-data"></a>

You can add custom data to game session and player session objects, which contain all the settings and metadata for a session\. Custom data is stored with Amazon GameLift and can be retrieved by other components as needed\. The Amazon GameLift service passes game session data to a game server when starting a new game session, and passes player session data to the game server when a player connects to the game session\. Custom game and player data is not used by Amazon GameLift; it can be formatted as needed for use by your game client, game server, or other game services\. 

Game data may be useful for a variety of reasons\. For example, when matching prospective players to game sessions, your game might use game properties to inform a best\-match algorithm or help players choose from a list of game sessions\. Alternatively, you might use game properties to pass information that a game server needs when setting up a new game session, such as a game mode or map\. 

Player data has a range of uses as well\. For example, a matchmaking service might use player data to select a best match or team placement\. A game server might customize a player's experience based on their guild membership\.  

### Filter and sort available game sessions<a name="game-sessions-intro-features-search"></a>

Use session search and sort to find the best match for a prospective player or allow players to browse a list of available game sessions\. With this feature, you can effectively lead players to sessions that are most likely to result in a positive gaming experience\. For example, if your game requires a minimum number of players, directing new players into nearly\-filled games will minimize wait time for all players\. Alternatively, you'll likely want to hide sessions that are nearly finished\. Session search can be very useful for implementing a "join now" feature backed by a well\-formulated search and sort expression that gets players into positive gaming experiences fast\. Use session search and sort to find game sessions based on characteristics like session age, available player slots, current player count, maximum players allowed, and custom game session data\. You can also search and sort based on your own custom game data\.

### Track game and player usage data<a name="game-sessions-intro-features-track"></a>

Have Amazon GameLift automatically store logs for completed game sessions\. Set up log storage when integrating Amazon GameLift into your game servers\. You can access stored logs by downloading them through the Amazon GameLift console or programmatically with the AWS SDK for Amazon GameLift\.

Use the Amazon GameLift console to view detailed information on game sessions, including session metadata and settings as well as player session data\. For each game session, you can view a list of player sessions along with total times played\. You can also view metrics data and graphs that track the number of active game sessions and player sessions over time\. See more information at [View Data on Game and Player Sessions](gamelift-console-game-player-sessions-metrics.md) and [Metrics](gamelift-console-fleets-metrics.md#fleets-metrics-tab)\.