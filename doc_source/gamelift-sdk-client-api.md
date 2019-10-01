# Add Amazon GameLift to Your Game Client<a name="gamelift-sdk-client-api"></a>

You can integrate Amazon GameLift into game components that need to acquire game session information, create new game sessions, and/or join players to games\. Depending on your game architecture, this functionality might be placed in the game client or in game services that handle tasks such as player authentication, matchmaking, or game placement\.

To do this, use the AWS SDK with Amazon GameLift API\. This SDK is available in C\+\+, C\#, and several other languages\. For details on the AWS SDK, version information, and language support, see [For Client Services](gamelift-supported.md#gamelift-supported-clients)\. Most of the links in this topic go to the *[Amazon GameLift Service API Reference](https://docs.aws.amazon.com/gamelift/latest/apireference/)*, which describes the low\-level service API for Amazon GameLift\-related actions and includes links to language\-specific reference guides\. 

**Note**  
Interested in using matchmaking to get players into games? Once you've set up Amazon GameLift on your game client or a service, add FlexMatch to match players and create custom game sessions\. See more at [Adding FlexMatch Matchmaking](match-intro.md)\. 

## Set Up Amazon GameLift on a Client or Service<a name="gamelift-sdk-client-api-initialize"></a>

You add code to initialize an Amazon GameLift client and store some key settings for use with Amazon GameLift\. This code needs to be located so that it runs before any Amazon GameLift\-dependent code, such as on launch\. 

**Note**  
To set up your game client for testing using Amazon GameLift Local, see [Testing Your Integration](integration-testing-local.md)

1. Decide whether to use the default client configuration or create custom settings\. For custom settings, you must create a custom `ClientConfiguration` object\. See [AWS ClientConfiguration](http://sdk.amazonaws.com/cpp/api/LATEST/struct_aws_1_1_client_1_1_client_configuration.html) \(C\+\+\) for object structure and the default settings\.

   A client configuration specifies a target region and endpoint\. The region determines which resources \(fleets, queues, matchmakers, etc\.\) Amazon GameLift interacts with when responding to requests\. The default client configuration specifies the US East \(N\. Virginia\) region\. To use any other region, create a custom configuration\. See this [list of AWS regions supported by Amazon GameLift](https://docs.aws.amazon.com/general/latest/gr/rande.html#gamelift_region) for names and endpoints\. If your client or service needs to make requests for multiple regions, create a separate client configuration object for each target region and each as needed\. See [Using Regions with the AWS SDKs](https://aws.amazon.com/articles/3912) for language\-specific examples\.

1. Initialize an Amazon GameLift client\. Call [Aws::GameLift::GameLiftClient\(\)](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_game_lift_1_1_game_lift_client.html) \(C\+\+\) using either a client configuration with the default settings or a custom configuration\. 

1. Add a mechanism to generate a unique identifier for each player\. Amazon GameLift requires a unique player ID to connect to a game session\. For more details, see [Generate Player IDs](player-sessions-player-identifiers.md)\.

1. Collect and store the following information to use when contacting Amazon GameLift:
   + **Target fleet** – Most Amazon GameLift API requests must specify a fleet, such as when getting information on available game sessions or managing game sessions and player sessions\. How you define the optimal target fleet \(for example, setting a static fleet, or choosing a fleets based on a device's physical location\)\. To specify a target fleet, use either a fleet ID or an alias ID that points to the target fleet\. Fleet aliases are highly useful in that you can switch players from one fleet to another without issuing a game client update\. The combination of target fleet and region \(specified in the client configuration\) uniquely identifies the fleet\. 
   + **Target queue** – If your game uses multi\-fleet queues to place new game sessions, you can specify which queue to use\. To specify a target queue, use the queue name\. The queue must be configured in the region 
   + **AWS credentials** – All calls to the Amazon GameLift service must provide credentials for the AWS account that hosts the game\. This is the account you used to set up your Amazon GameLift fleets, and you should have created an [IAM user or user group for players](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-iam-policy-intro.html) with a permissions policy\. You need to create an [Aws::Auth::AWSCredentials](http://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_auth_1_1_a_w_s_credentials.html) \(C\+\+\) object containing an IAM access key and secret key for the player user group\. For help finding the keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)\. 

## Get Game Sessions<a name="gamelift-sdk-client-api-find"></a>

Add code to discover available game sessions and manage game sessions settings and metadata\. See [Game and Player Session Features](game-sessions-intro.md#game-sessions-intro-features) for more on game session features\.

**Search for active game sessions\.** 

Use [SearchGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_SearchGameSessions.html) to get information on a specific game session, all active sessions, or sessions that meet a set of search criteria\. This call returns a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object for each active game session that matches your search request\.

Use search criteria to get a filtered list of active game sessions for players to join\. For example, you can filter sessions as follows: 
+ Exclude game sessions that are not accepting new players: `PlayerSessionCreationPolicy = DENY_ALL`
+ Exclude game sessions that are full: `CurrentPlayerSessionCount = MaximumPlayerSessionCount`
+ Choose game sessions based on length of time the session has been running: Evaluate `CreationTime`
+ Find game sessions based on a custom game property: `gameSessionProperties.gameMode = "brawl"`

**Manage game sessions\.**

Use any of the following operations to retrieve or update game session information\.
+ [DescribeGameSessionDetails\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionDetails.html) – Get a game session's protection status in addition to game session information\. 
+ [UpdateGameSession\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html) – Change a game session's metadata and settings as needed\.
+ [GetGameSessionLogUrl](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetGameSessionLogUrl.html) – Access stored game session logs\. 

## Create Game Sessions<a name="gamelift-sdk-client-api-create"></a>

Add code to start new game sessions on your deployed fleets and make them available to players\. There are two options for creating game sessions, depending on whether you are deploying your game in multiple regions or in a single region\. 

**Create a game session using a multi\-region queue\.** 

Use [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html) to place a request for a new game session in a queue\. To use this feature, you'll need to set up a queue, which determines where \(on which fleets\) the new game session can be placed\. A queue processes a game session placement request by trying each possible fleet, in turn, until it finds one with available resources to host the new game session\. For more detailed information on queues and how to use them, see  [Design a Game Session Queue](queues-design.md)\.

When creating a game session placement, specify the name of the queue to use, a game session name, a maximum number of concurrent players for the game, and an optional set of game properties\. You can optionally provide a list of players to automatically join to the game session\. If you include player latency data for relevant regions, Amazon GameLift uses this information to place the new game session on a fleet that provides the best possible gameplay experience for players\. 

Game session placement is an asynchronous process\. Once you've placed a request, you can let it succeed or time out\. You can also cancel the request at any time using [StopGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopGameSessionPlacement.html)\. To check the status of your placement request, call [DescribeGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionPlacement.html) to retrieve an updated GameSessionPlacement object\. When a new game session is created, the GameSessionPlacement reflects the following changes: \(1\) Status changes from Pending to Fulfilled; \(2\) New game session information is added, including game session ID and region; and \(3\) New player session information is added \(if requested\)\. 

**Create a game session on a specific fleet\.** 

Use [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html) to create a new session on a specified fleet\. This synchronous operation succeeds or fails depending on whether the fleet has resources available to host a new game session\. Your game should handle failures as best suits your game and players\. For example, you might repeat the request until resources are freed or scaled up, or you might switch to a different fleet\. Once Amazon GameLift has created the new game session and returned a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object, you can start joining players to it\.

When you use this method to create a game session, specify a fleet ID or alias ID, a session name, and a maximum number of concurrent players for the game\. Optionally, you can include a set of game properties\. Game properties are defined in an array of key–value pairs in which you define the keys and a set of values that are meaningful to your game\. This information is passed to the server process hosting the new game session, to be used as designed in your game server\. For example, you might use game properties to direct the game session to use a certain game map or a particular set of rules\.

If you use the Amazon GameLift resource protection feature to limit the number of game sessions one player can create, you'll need to specify the game session creator's player ID\. 

## Join a Player to a Game Session<a name="gamelift-sdk-client-api-join"></a>

Add code to reserve player slots in active game sessions and connect game clients to game sessions\. 

1. **Reserve a player slot in a game session\.** 

   To reserve a player slot, create a new player session for the game session\. See [How Players Connect to Games](game-sessions-intro.md) for more on player sessions\. You have two ways to create new player sessions:
   + If you're using `StartGameSessionPlacement` to create game sessions, as described in the previous section, you can reserve slots for one or more players in the new game session\. 
   + Reserve player slots for one or more players using [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html) or [CreatePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSessions.html) with a game session ID\.

   With both methods, Amazon GameLift first verifies that the game session is accepting new players and has an available player slot\. If successful, Amazon GameLift reserves a slot for the player, creates the new player session, and returns a [PlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_PlayerSession.html) object containing the DNS name, IP address, and port that a game client needs to connect to the game session\.

   A player session request must include a unique ID for each player\. See [Generate Player IDs](player-sessions-player-identifiers.md) for more on player IDs\.

   Optionally, a player session request can include a set of custom player data\. This data is stored in the newly created player session object, which can be retrieved by calling [DescribePlayerSessions\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html)\. It is also passed from the Amazon GameLift service to the game server when the player connects directly to the game session\. Player data is not used by Amazon GameLift; it is a simple string of characters that is available to your game components for interpretation\. When requesting multiple player sessions, you can provide a string of player data for each player, mapped to the player ID in the request\.

1. **Connect to a game session\.**

   Add code to the game client to retrieve the `PlayerSession` object, which contains the game session's connection information\. Use this information to establish a direct connection to the server process\. 
   + You can connect using the specified port and either the DNS name or IP address assigned to the server process\. 
**Note**  
If your fleets have TLS certificate generation enabled, you must connect using the DNS name and port\. This is required even if you haven't implemented a server authentication process\. 
   + If you're using player session IDs to reserve player slots and track player connections, you must reference the player session ID\. 

   Once connected, the game client and server process communicate directly without involving the GameLift service to communicate gameplay\. The server process maintains communication with the GameLift service to report player connection status, health status, etc\. On initial connection, it may contact the service to verify that the player session ID is valid and in a reserved status\. If validated, the reservation is claimed and the player connection is accepted\. Later, when the player disconnects, the server process reports the dropped connection\.

   If you want to enable server authentication and encrypt data packets travelling between game client and the game session, you need to build this functionality\. When the TLS certificate generation feature is enabled for a new fleet, GameLift only gets the TLS certificate and creates DNS entries for each instance in the fleet\.