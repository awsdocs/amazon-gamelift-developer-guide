# GameLift server API reference for C\+\+: Data types<a name="integration-server-sdk5-cpp-datatypes"></a>

This GameLift C\+\+ Server API reference can help you prepare your multiplayer game for use with GameLift\. For details about the integration process, see [Add GameLift to your game server](gamelift-sdk-server-api.md)\.

**Topics**
+ [LogParameters](#integration-server-sdk5-cpp-dataypes-log)
+ [ProcessParameters](#integration-server-sdk5-cpp-dataypes-process)
+ [UpdateGameSession](#integration-server-sdk5-cpp-dataypes-updategamesession)
+ [GameSession](#integration-server-sdk5-cpp-dataypes-gamesession)
+ [ServerParameters](#integration-server-sdk5-cpp-dataypes-serverparameters)
+ [StartMatchBackfillRequest](#integration-server-sdk5-cpp-dataypes-startmatchbackfillrequest)
+ [Player](#integration-server-sdk5-cpp-dataypes-player)
+ [DescribePlayerSessionsRequest](#integration-server-sdk5-cpp-dataypes-playersessions)
+ [StopMatchBackfillRequest](#integration-server-sdk5-cpp-dataypes-stopmatchbackfillrequest)
+ [GetCustomerRoleCredentialsRequest](#integration-server-sdk5-cpp-dataypes-getcustomerrolecredentialsrequest)

## LogParameters<a name="integration-server-sdk5-cpp-dataypes-log"></a>

An object identifying files generated during a game session that you want GameLift to upload and store after the game session ends\. The game server provides `LogParameters` to GameLift as part of a `ProcessParameters` object in a [ProcessReady\(\)](integration-server-sdk5-cpp-actions.md#integration-server-sdk5-cpp-processready) call\.


|  |  | 
| --- |--- |
|  **Properties**  | Description | 
| LogPaths |  The list of directory paths to game server log files you want GameLift to store for future access\. The server process generates these files during each game session\. You define file paths and names in your game server and store them in the root game build directory\.  The log paths must be absolute\. For example, if your game build stores game session logs in a path like `MyGame\sessionLogs\`, then the path would be `c:\game\MyGame\sessionLogs` on a Windows instance\. **Type:** `std:vector<std::string>` **Required:** No  | 

## ProcessParameters<a name="integration-server-sdk5-cpp-dataypes-process"></a>

This data type contains the set of parameters sent to GameLift in a [ProcessReady\(\)](integration-server-sdk5-cpp-actions.md#integration-server-sdk5-cpp-processready)\.


|  |  | 
| --- |--- |
|  **Properties**  | Description | 
| LogParameters | An object with directory paths to files that are generated during a game session\. GameLift copies and stores the files for future access\.**Type:** `Aws::GameLift::Server::LogParameters`**Required:** No | 
| OnHealthCheck | The callback function that GameLift invokes to request a health status report from the server process\. GameLift calls this function every 60 seconds and waits 60 seconds for a response\. The server process returns TRUE if healthy, FALSE if not healthy\. If no response is returned, GameLift records the server process as not healthy\.**Type:** `std::function<bool()> onHealthCheck`**Required:** No | 
| OnProcessTerminate | The callback function that GameLift invokes to force the server process to shut down\. After calling this function, GameLift waits 5 minutes for the server process to shut down and respond with a [ProcessEnding\(\)](integration-server-sdk5-cpp-actions.md#integration-server-sdk5-cpp-processending) call before it shuts down the server process\.**Type:** `std::function<void()> onProcessTerminate`**Required:** Yes | 
| OnRefreshConnection | The name of the callback function that GameLift invokes to refresh the connection with the game server\.**Type:** `void OnRefreshConnectionDelegate()`**Required:** Yes | 
| OnStartGameSession | The callback function that GameLift invokes to activate a new game session\. GameLift calls this function in response to a client request [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html)\. The callback function passes a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object, as defined in the GameLift API Reference\.**Type:** `const std::function<void(Aws::GameLift::Model::GameSession)> onStartGameSession`**Required:** Yes | 
| OnUpdateGameSession | The callback function that GameLift invokes to pass an updated game session object to the server process\. GameLift calls this function when a match backfill request has been processed to provide updated matchmaker data\. It passes a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object, a status update \(updateReason\), and the match backfill ticket ID\.**Type:** `std::function<void(Aws::GameLift::Server::Model::UpdateGameSession)> onUpdateGameSession`**Required:** No | 
| Port | The port number the server process listens on for new player connections\. The value must fall into the port range configured for any fleet deploying this game server build\. This port number is included in game session and player session objects, which game sessions use when connecting to a server process\.**Type:** `Integer`**Required:** Yes | 

## UpdateGameSession<a name="integration-server-sdk5-cpp-dataypes-updategamesession"></a>

This data type updates to a game session object, which includes the reason that the game session was updated and the related backfill ticket ID if backfill is used to fill player sessions in the game session\.


| Properties | **Description** | 
| --- | --- | 
| GameSession | A [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object defined by the GameLift API\. The GameSession object contains properties describing a game session\. **Type:** `Aws::GameLift::Server::GameSession`**Required:** Yes | 
| UpdateReason | The reason that the game session is being updated\.**Type:** `Aws::GameLift::Server::UpdateReason`**Required:** Yes | 
| BackfillTicketId | The ID of the backfill ticket attempting to update the game session\.**Type:** `std::string`**Required:** No | 

## GameSession<a name="integration-server-sdk5-cpp-dataypes-gamesession"></a>

This data type provides details of a game session\. 


| Properties | **Description** | 
| --- | --- | 
| GameSessionId |  A unique identifier for the game session\. A game session ARN has the following format: `arn:aws:gamelift:<region>::gamesession/<fleet ID>/<custom ID string or idempotency token>`\. **Type:** `std::string` **Required**: No  | 
| Name |  A descriptive label of the game session\.  **Type:** `std::string` **Required**: No  | 
| FleetId |  A unique identifier for the fleet that the game session is running on\. **Type:** `std::string` **Required**: No  | 
| MaximumPlayerSessionCount |  The maximum number of player connections to the game session\. **Type:** `int` **Required**: No  | 
| Port |  The port number for the game session\. To connect to a GameLift game server, an app needs both the IP address and port number\. **Type:** `in` **Required**: No  | 
| IpAddress |  The IP address of the game session\. To connect to a GameLift game server, an app needs both the IP address and port number\. **Type:** `std::string` **Required**: No  | 
| GameSessionData |  A set of custom game session properties, formatted as a single string value\.  **Type:** `std::string` **Required**: No  | 
| MatchmakerData |  Information about the matchmaking process that was used to create the game session, in JSON syntax, formatted as a string\. In addition to the matchmaking configuration used, it contains data on all players assigned to the match, including player attributes and team assignments\. **Type:** `std::string` **Required**: No  | 
| GameProperties |  A set of custom properties for a game session, formatted as key:value pairs\. These properties are passed with a request to start a new game session\. **Type:** `std :: vector < GameProperty >` **Required**: No  | 
| DnsName |  The DNS identifier assigned to the instance that's running the game session\. Values have the following format: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk5-cpp-datatypes.html) When connecting to a game session that's running on a TLS\-enabled fleet, you must use the DNS name, not the IP address\. **Type:** `std::string` **Required**: No  | 

## ServerParameters<a name="integration-server-sdk5-cpp-dataypes-serverparameters"></a>

An object containing information about a GameLift Anywhere server\. The server communicates this information to the GameLift service when launching a new server process with [InitSDK\(\)](integration-server-sdk5-cpp-actions.md#integration-server-sdk5-cpp-initsdk)\. This parameter isn't used with services hosted on GameLift managed EC2 instances\. 


| Properties | **Description** | 
| --- | --- | 
| webSocketUrl |  The `GameLiftServerSdkEndpoint` GameLift returns when you [https://docs.aws.amazon.com/RegisterCompute](https://docs.aws.amazon.com/RegisterCompute) for a GameLift Anywhere compute resource\. **Type:** `std::string` **Required**: Yes   | 
| processId |  A unique identifier registered to the server process hosting your game\. **Type:** `std::string` **Required**: Yes  | 
| hostId | The HostID is the ComputeName used when you registered your compute\. For more information see, [RegisterCompute](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html)\.**Type:** `std::string`**Required**: Yes | 
| fleetId | The unique identifier of the fleet that the compute is registered to\. For more information see, [RegisterCompute](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html)\.**Type:** `std::string`**Required**: Yes | 
| authToken | The authentication token generated by GameLift that authenticates your server to GameLift\. For more information see, [GetComputeAuthToken](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html)\.**Type:** `std::string`**Required**: Yes | 

## StartMatchBackfillRequest<a name="integration-server-sdk5-cpp-dataypes-startmatchbackfillrequest"></a>

This data type cancels a matchmaking backfill request\. The game server communicates this information to GameLift in a [StopMatchBackfill\(\)](integration-server-sdk5-cpp-actions.md#integration-server-sdk5-cpp-stopmatchbackfill) call\.


| Properties | **Description** | 
| --- | --- | 
| GameSessionArn |  A unique game session identifier\. The API operation `[GetGameSessionId](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk5-cpp-actions.html#integration-server-sdk5-cpp-getgamesessionid)` returns the identifier in ARN format\. **Type:** `std::string` **Required**: Yes  | 
| MatchmakingConfigurationArn |  A unique identifier, in the form of an ARN, for the matchmaker to use for this request\. The matchmaker ARN for the original game session is in the game session object in the matchmaker data property\. Learn more about matchmaker data in [Work with matchmaker data](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-server.html#match-server-data.html)\. **Type:** `std::string` **Required**: Yes  | 
| Players |  A set of data representing all players who are in the game session\. The matchmaker uses this information to search for new players who are good matches for the current players\. **Type:** `std::vector<Player>` **Required**: Yes  | 
| TicketId |  A unique identifier for a matchmaking or match backfill request ticket\. If you don't provide a value, GameLift generates one\. Use this identifier to track the match backfill ticket status or cancel the request if needed\.  **Type:** `std::string` **Required**: No  | 

## Player<a name="integration-server-sdk5-cpp-dataypes-player"></a>

This data type represents a player in matchmaking\. When starting a matchmaking request, a player has a player ID, attributes, and possibly latency data\. GameLift adds team information after a match is made\.


| Properties | **Description** | 
| --- | --- | 
| LatencyInMS |  A set of values expressed in milliseconds, that indicate the amount of latency that a player experiences when connected to a location\.  If this property is used, the player is only matched for locations listed\. If a matchmaker has a rule that evaluates player latency, players must report latency to be matched\. **Type:** `Dictionary<string,int>` **Required**: No  | 
| PlayerAttributes |  A collection of key:value pairs containing player information for use in matchmaking\. Player attribute keys must match the PlayerAttributes used in a matchmaking rule set\. For more information about player attributes, see [AttributeValue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_AttributeValue.html)\. **Type:** `std::map<std::string,AttributeValue>` **Required**: No  | 
| PlayerId |  A unique identifier for a player\. **Type:** `std::string` **Required**: No  | 
| Team |  The name of the team that the player is assigned to in a match\. You define team name in the matchmaking rule set\. **Type:** `std::string` **Required**: No  | 

## DescribePlayerSessionsRequest<a name="integration-server-sdk5-cpp-dataypes-playersessions"></a>

An object that specifies which player sessions to retrieve\. The server process provides this information with a [DescribePlayerSessions\(\)](integration-server-sdk5-cpp-actions.md#integration-server-sdk5-cpp-describeplayersessions) call to GameLift\.


| Properties | **Description** | 
| --- | --- | 
| GameSessionId |  A unique game session identifier\. Use this parameter to request all player sessions for the specified game session\.  Game session ID format is `arn:aws:gamelift:<region>::gamesession/fleet-<fleet ID>/<ID string>`\. The `GameSessionID` is a custom ID string or a **Type:** `std::string` **Required**: No  | 
| PlayerSessionId |  The unique identifier for a player session\. Use this parameter to request a single specific player session\. **Type:** `std::string` **Required**: No  | 
| PlayerId |  The unique identifier for a player\. Use this parameter to request all player sessions for a specific player\. See [Generate player IDs](player-sessions-player-identifiers.md)\. **Type:** `std::string` **Required**: No  | 
| PlayerSessionStatusFilter |  The player session status to filter results on\. Possible player session statuses include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk5-cpp-datatypes.html) **Type:** `std::string` **Required**: No  | 
| NextToken |  The token indicating the start of the next page of results\. To specify the start of the result set, don't provide a value\. If you provide a player session ID, this parameter is ignored\. **Type:** `std::string` **Required**: No  | 
| Limit |  The maximum number of results to return\. If you provide a player session ID, this parameter is ignored\. **Type:** `int` **Required**: No  | 

## StopMatchBackfillRequest<a name="integration-server-sdk5-cpp-dataypes-stopmatchbackfillrequest"></a>

This data type is used to cancel a matchmaking backfill request\. The information is communicated to the GameLift service in a [StopMatchBackfill\(\)](integration-server-sdk5-cpp-actions.md#integration-server-sdk5-cpp-stopmatchbackfill) call\.


| Properties | **Description** | 
| --- | --- | 
| GameSessionArn |  A unique game session identifier of the request being canceled\. **Type:** `char[]` **Required**: No  | 
| MatchmakingConfigurationArn |  A unique identifier of the matchmaker this request was sent to\. **Type:** `char[]` **Required**: No  | 
| TicketId |  A unique identifier of the backfill request ticket to be canceled\. **Type:** `char[]` **Required**: No  | 

## GetCustomerRoleCredentialsRequest<a name="integration-server-sdk5-cpp-dataypes-getcustomerrolecredentialsrequest"></a>

This data type provides role credentials that extend limited access to your AWS resources to the game server\. For more information see, [Set up an IAM service role for GameLift](setting-up-role.md)\.


| Properties | **Description** | 
| --- | --- | 
| RoleArn | The Amazon Resource Name \(ARN\) of the service role that extends limited access to your AWS resources\.**Type:** `char[]`**Required**: No | 
| RoleSessionName | The name of the session describing the use of the role credentials\.**Type:** `char[]`**Required**: No | 