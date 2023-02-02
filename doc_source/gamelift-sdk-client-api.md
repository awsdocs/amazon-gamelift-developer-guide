# Add GameLift to your game client<a name="gamelift-sdk-client-api"></a>

Integrate Amazon GameLift into game components that need game session information, create new game sessions, and add players to games\. Depending on your game architecture, this functionality is in backend services that handle tasks such as player authentication, matchmaking, or game session placement\.

**Note**  
For detailed information about how to set up matchmaking for your GameLift hosted game, see the [GameLift FlexMatch Developer Guide](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide)\.

## Set up GameLift on a backend service<a name="gamelift-sdk-client-api-initialize"></a>

Add code to initialize a game client and store key settings for use with GameLift\. This code must run before any code dependent on GameLift\.

1. Use the default client configuration or create a custom client configuration object\. For more information, see [https://sdk.amazonaws.com/cpp/api/LATEST/struct_aws_1_1_client_1_1_client_configuration.html](https://sdk.amazonaws.com/cpp/api/LATEST/struct_aws_1_1_client_1_1_client_configuration.html) \(C\+\+\) or [AmazonGameLiftConfig](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/GameLift/TGameLiftConfig.html) \(C\#\)\.

   A client configuration specifies a target location and endpoint\. The location determines which resources \(such as fleets, queues, and matchmakers\) GameLift interacts with when responding to requests\. The default client configuration specifies the US East \(N\. Virginia\) Region\. To use any other location, create a custom configuration\.

1. Initialize a GameLift client\. Use [Aws::GameLift::GameLiftClient\(\)](https://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_game_lift_1_1_game_lift_client.html) \(C\+\+\) or [AmazonGameLiftClient\(\)](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/GameLift/TGameLiftClient.html) \(C\#\) with a default client configuration or a custom client configuration\.

1. Add a mechanism to generate a unique identifier for each player\. For more information, see [Generate player IDs](player-sessions-player-identifiers.md)\.

1. Collect and store the following information to use when contacting GameLift:
   + **Target fleet** – Most GameLift API requests must specify a target fleet\. To do so, use either a fleet ID or an alias ID that points to the target fleet\. With fleet aliases, you can switch players from one fleet to another without issuing a game client update\.
   + **Target queue** – If your game uses multi\-fleet queues to place new game sessions, then you can specify which queue to use\. To specify a target queue, use the queue name\.
   + **AWS credentials** – All calls to GameLift must provide credentials for the AWS account that hosts the game\. This is the same account that you used to set up your GameLift fleets\. To provide access to your backend servers, create an [AWS Identity and Access Management \(IAM\) user or user group for players](gamelift-iam-policy-examples.md#iam-policy-admin-game-dev-example)\. Also create an [Aws::Auth::AWSCredentials](https://sdk.amazonaws.com/cpp/api/LATEST/class_aws_1_1_auth_1_1_a_w_s_credentials.html) \(C\+\+\) object containing an IAM access key and secret key for the player user group\. For help with finding the keys, see [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

## Get game sessions<a name="gamelift-sdk-client-api-find"></a>

Add code to discover available game sessions and manage game session settings and metadata\.

**Search for active game sessions**

Use [SearchGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_SearchGameSessions.html) to get information about a specific game session, all active sessions, or sessions that meet a set of search criteria\. This call returns a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object for each active game session that matches your search request\.

Use search criteria to get a filtered list of active game sessions for players to join\. For example, you can filter sessions as follows:
+ Exclude game sessions that are full: `CurrentPlayerSessionCount = MaximumPlayerSessionCount`\.
+ Choose game sessions based on length of time that the session has been running: Evaluate `CreationTime`\.
+ Find game sessions based on a custom game property: `gameSessionProperties.gameMode = "brawl"`\.

**Manage game sessions**

Use any of the following operations to retrieve or update game session information\.
+ [DescribeGameSessionDetails\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionDetails.html) – Get a game session's protection status in addition to game session information\.
+ [UpdateGameSession\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html) – Change a game session's metadata and settings as needed\.
+ [GetGameSessionLogUrl](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetGameSessionLogUrl.html) – Access stored game session logs\.

## Create game sessions<a name="gamelift-sdk-client-api-create"></a>

Add code to start new game sessions on your deployed fleets and make them available to players\. There are two options for creating game sessions, depending on whether you're deploying your game in multiple AWS Regions or in a single Region\.

**Create a game session in a multi\-location queue**

Use [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html) to place a request for a new game session in a queue\. To use this operation, create a queue\. This determines where GameLift places the new game session\. For more information about queues and how to use them, see [Setting up GameLift queues for game session placement](queues-intro.md)\.

When creating a game session placement, specify the name of the queue to use, a game session name, a maximum number of concurrent players, and an optional set of game properties\. You can also optionally provide a list of players to automatically join the game session\. If you include player latency data for relevant Regions, then GameLift uses this information to place the new game session on a fleet that provides the ideal gameplay experience for the players\.

Game session placement is an asynchronous process\. After you've placed a request, you can let it succeed or time out\. You can also cancel the request at any time using [StopGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopGameSessionPlacement.html)\. To check the status of your placement request, call [DescribeGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionPlacement.html)\.

**Create a game session on a specific fleet**

Use [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html) to create a new session on a specified fleet\. This synchronous operation succeeds or fails depending on whether the fleet has resources available to host a new game session\. After GameLift creates the new game session and returns a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object, you can join players to it\.

When you use this operation, provide a fleet ID or alias ID, a session name, and the maximum number of concurrent players for the game\. Optionally, you can include a set of game properties\. Game properties are defined in an array of key\-value pairs\.

If you use the GameLift resource protection feature to limit the number of game sessions that one player can create, then provide the game session creator's player ID\.

## Join a player to a game session<a name="gamelift-sdk-client-api-join"></a>

Add code to reserve a player slot in an active game session and connect game clients to game sessions\.

1. 

**Reserve a player slot in a game session**

   To reserve a player slot, create a new player session for the game session\. For more information about player sessions, see [How players connect to games](game-sessions-intro.md)\.

   There are two ways to create new player sessions:
   + Use [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html) to reserve slots for one or more players in the new game session\.
   + Reserve player slots for one or more players using [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html) or [CreatePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSessions.html) with a game session ID\.

   GameLift first verifies that the game session is accepting new players and has available player slots\. If successful, GameLift reserves a slot for the player, creates the new player session, and returns a [PlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_PlayerSession.html) object\. This object contains the DNS name, IP address, and port that a game client needs to connect to the game session\.

   A player session request must include a unique ID for each player\. For more information, see [Generate player IDs](player-sessions-player-identifiers.md)\.

   A player session can include a set of custom player data\. This data is stored in the newly created player session object, which you can retrieve by calling [DescribePlayerSessions\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html)\. GameLift also passes this object to the game server when the player connects directly to the game session\. When requesting multiple player sessions, provide a string of player data for each player that's mapped to the player ID in the request\.

1. 

**Connect to a game session**

   Add code to the game client to retrieve the `PlayerSession` object, which contains the game session's connection information\. Use this information to establish a direct connection to the server\.
   + You can connect using the specified port and the DNS name or IP address assigned to the server process\.
   + If your fleets have TLS certificate generation enabled, then connect using the DNS name and port\.
   + If your game server validates incoming player connections, then reference the player session ID\.

   After making the connection, the game client and server process communicate directly without involving GameLift\. The server maintains communication with GameLift to report player connection status, health status, and more\. If the game server validates incoming players, then it verifies that the player session ID matches a reserved slot in the game session, and accepts or denies the player connection\. When the player disconnects, the server process reports the dropped connection\.