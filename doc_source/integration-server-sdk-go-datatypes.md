# GameLift server SDK \(Go\) reference: Data types<a name="integration-server-sdk-go-datatypes"></a>

You can use this GameLift Go server SDK reference to help you prepare your multiplayer game for use with GameLift\. For details about the integration process, see [Add GameLift to your game server](gamelift-sdk-server-api.md)\.

**Topics**
+ [LogParameters](#integration-server-sdk-go-dataypes-log)
+ [ProcessParameters](#integration-server-sdk-go-dataypes-process)
+ [UpdateGameSession](#integration-server-sdk-go-dataypes-updategamesession)
+ [GameSession](#integration-server-sdk-go-dataypes-gamesession)
+ [ServerParameters](#integration-server-sdk-go-dataypes-serverparameters)
+ [StartMatchBackfillRequest](#integration-server-sdk-go-dataypes-startmatchbackfillrequest)
+ [Player](#integration-server-sdk-go-dataypes-player)
+ [DescribePlayerSessionsRequest](#integration-server-sdk-go-dataypes-playersessions)
+ [StopMatchBackfillRequest](#integration-server-sdk-go-dataypes-stopmatchbackfillrequest)
+ [GetFleetRoleCredentialsRequest](#integration-server-sdk-go-dataypes-getfleetrolecredentialsrequest)

## LogParameters<a name="integration-server-sdk-go-dataypes-log"></a>

An object identifying files generated during a game session that you want GameLift to upload and store after the game session ends\. The game server provides `LogParameters` to GameLift as part of a `ProcessParameters` object in a [ProcessReady\(\)](integration-server-sdk-go-actions.md#integration-server-sdk-go-processready) call\.


|  |  | 
| --- |--- |
|  **Properties**  | Description | 
| LogPaths |  The list of directory paths to game server log files that you want GameLift to store for future access\. The server process generates these files during each game session\. You define file paths and names in your game server and store them in the root game build directory\.  The log paths must be absolute\. For example, if your game build stores game session logs in a path such as `MyGame\sessionLogs\`, then the path would be `c:\game\MyGame\sessionLogs` on a Windows instance\. **Type:** `[]string` **Required:** No  | 

## ProcessParameters<a name="integration-server-sdk-go-dataypes-process"></a>

An object describing the communication between a server process and GameLift\. The server process provides this information to GameLift with a call to [ProcessReady\(\)](integration-server-sdk-go-actions.md#integration-server-sdk-go-processready)\.


|  |  | 
| --- |--- |
|  **Properties**  | Description | 
| LogParameters | An object with directory paths to files that are generated during a game session\. GameLift copies and stores the files for future access\.**Type:** `LogParameters`**Required:** No | 
| OnHealthCheck | The callback function that GameLift invokes to request a health status report from the server process\. GameLift calls this function every 60 seconds and waits 60 seconds for a response\. The server process returns TRUE if healthy, FALSE if not healthy\. If no response is returned, GameLift records the server process as not healthy\.**Type:** `OnHealthCheck func() bool`**Required:** No | 
| OnProcessTerminate | The callback function that GameLift invokes to force the server process to shut down\. After calling this function, GameLift waits 5 minutes for the server process to shut down and respond with a [ProcessEnding\(\)](integration-server-sdk-go-actions.md#integration-server-sdk-go-processending) call before it shuts down the server process\.**Type:** `OnProcessTerminate func()`**Required:** Yes | 
| OnStartGameSession | The callback function that GameLift invokes to pass an updated game session object to the server process\. GameLift calls this function when a match backfill request has been processed to provide updated matchmaker data\. It passes a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object, a status update \(updateReason\), and the match backfill ticket ID\.**Type:** `OnStartGameSession func (model.GameSession )`**Required:** Yes | 
| OnUpdateGameSession | The callback function that GameLift invokes to pass updated game session information to the server process\. GameLift calls this function after processing a match backfill request to provide updated matchmaker data\. **Type:** `OnUpdateGameSession func (model.UpdateGameSession)`**Required:** No | 
| Port | The port number that the server process listens on for new player connections\. The value must fall into the port range configured for any fleet deploying this game server build\. This port number is included in game session and player session objects, which game sessions use when connecting to a server process\.**Type:** `int`**Required:** Yes | 

## UpdateGameSession<a name="integration-server-sdk-go-dataypes-updategamesession"></a>

The updates to a game session object, which includes the reason that the game session was updated, and the related backfill ticket ID if backfill is being used to fill player sessions in the game session\.


| Properties | **Description** | 
| --- | --- | 
| GameSession | A [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object defined by the GameLift API\. The GameSession object contains properties describing a game session\. **Type:** `GameSession GameSession()`**Required:** Yes | 
| UpdateReason | The reason that the game session is being updated\.**Type:** `UpdateReason UpdateReason()`**Required:** Yes | 
| BackfillTicketId | The ID of the backfill ticket attempting to update the game session\.**Type:** `String`**Required:** No | 

## GameSession<a name="integration-server-sdk-go-dataypes-gamesession"></a>

The details of a game session\. 


| Properties | **Description** | 
| --- | --- | 
| GameSessionId |  A unique identifier for the game session\. A game session Amazon Resource Name \(ARN\) has the following format: `arn:aws:gamelift:<region>::gamesession/<fleet ID>/<custom ID string or idempotency token>`\. **Type:** `String` **Required**: No  | 
| Name |  A descriptive label of the game session\.  **Type:** `String` **Required**: No  | 
| FleetId |  A unique identifier for the fleet that the game session is running on\. **Type:** `String` **Required**: No  | 
| MaximumPlayerSessionCount |  The maximum number of player connections to the game session\. **Type:** `Integer` **Required**: No  | 
| Port |  The port number for the game session\. To connect to a GameLift game server, an app needs both the IP address and port number\. **Type:** `Integer` **Required**: No  | 
| IpAddress |  The IP address of the game session\. To connect to a GameLift game server, an app needs both the IP address and port number\. **Type:** `String` **Required**: No  | 
| GameSessionData |  A set of custom game session properties, formatted as a single string value\.  **Type:** `String` **Required**: No  | 
| MatchmakerData |  The information about the matchmaking process that was used to create the game session, in JSON syntax, formatted as a string\. In addition to the matchmaking configuration used, it contains data on all players assigned to the match, including player attributes and team assignments\. **Type:** `String` **Required**: No  | 
| GameProperties |  A set of custom properties for a game session, formatted as key:value pairs\. These properties are passed with a request to start a new game session\. **Type:** `map[string] string` **Required**: No  | 
| DnsName |  The DNS identifier assigned to the instance that's running the game session\. Values have the following format: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk-go-datatypes.html) When connecting to a game session that's running on a TLS\-enabled fleet, you must use the DNS name, not the IP address\. **Type:** `String` **Required**: No  | 

## ServerParameters<a name="integration-server-sdk-go-dataypes-serverparameters"></a>

An object containing information about a GameLift Anywhere server\. The server communicates this information to the GameLift service when launching a new server process with [InitSDK\(\)](integration-server-sdk-go-actions.md#integration-server-sdk-go-initsdk)\. This parameter isn't used with services hosted on GameLift managed EC2 instances\.


| Properties | **Description** | 
| --- | --- | 
| WebSocketURL |  The `GameLiftServerSdkEndpoint` GameLift returns when you [https://docs.aws.amazon.com/RegisterCompute](https://docs.aws.amazon.com/RegisterCompute) for a GameLift Anywhere compute resource\. **Type:** `string` **Required**: Yes  | 
| ProcessID |  A unique identifier registered to the server process hosting your game\. **Type:** `string` **Required**: Yes  | 
| HostID |  The unique identifier of the compute resource that's hosting the new server process\.  The `HostID` is the `ComputeName` used when you registered your compute\. For more information, see [RegisterCompute](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html)\. **Type:** `string` **Required**: Yes  | 
| FleetID | The unique identifier of the fleet that the compute is registered to\. For more information, see [RegisterCompute](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html)\.**Type:** `string`**Required**: Yes | 
| AuthToken | The authentication token generated by GameLift that authenticates your server to GameLift\. For more information, see [GetComputeAuthToken](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html)\.**Type:** `string`**Required**: Yes | 

## StartMatchBackfillRequest<a name="integration-server-sdk-go-dataypes-startmatchbackfillrequest"></a>

The data type used to cancel a matchmaking backfill request\. The game server communicates this information to GameLift in a [StopMatchBackfill\(\)](integration-server-sdk-go-actions.md#integration-server-sdk-go-stopmatchbackfill) call\.


| Properties | **Description** | 
| --- | --- | 
| GameSessionArn |  The unique game session identifier\. The API operation `[GetGameSessionId](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk-go-actions.html#integration-server-sdk-go-getgamesessionid)` returns the identifier in ARN format\. **Type:** `String` **Required**: Yes  | 
| MatchmakingConfigurationArn |  The unique identifier \(in the form of an ARN\) for the matchmaker to use for this request\. The matchmaker ARN for the original game session is in the game session object in the matchmaker data property\. For more information about matchmaker data, see [Work with matchmaker data](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-server.html#match-server-data.html)\. **Type:** `String` **Required**: Yes  | 
| Players |  A set of data that represents all players who are currently in the game session\. The matchmaker uses this information to search for new players who are good matches for the current players\. **Type:** `[]model.Player` **Required**: Yes  | 
| TicketId |  The unique identifier for a matchmaking or match backfill request ticket\. If you don't provide a value, GameLift generates one\. Use this identifier to track the match backfill ticket status or cancel the request if needed\.  **Type:** `String` **Required**: No  | 

## Player<a name="integration-server-sdk-go-dataypes-player"></a>

The object that represents a player in matchmaking\. When a matchmaking request starts, a player has a player ID, attributes, and possibly latency data\. GameLift adds team information after a match is made\.


| Properties | **Description** | 
| --- | --- | 
| LatencyInMS |  A set of values expressed in milliseconds that indicate the amount of latency that a player experiences when connected to a location\.  If this property is used, the player is only matched for locations listed\. If a matchmaker has a rule that evaluates player latency, players must report latency to be matched\. **Type:** `map[string] int` **Required**: No  | 
| PlayerAttributes |  A collection of key:value pairs that contain player information for use in matchmaking\. Player attribute keys must match the PlayerAttributes used in a matchmaking rule set\. For more information about player attributes, see [AttributeValue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_AttributeValue.html)\. **Type:** `map[string] AttributeValue` **Required**: No  | 
| PlayerId |  A unique identifier for a player\. **Type:** `String` **Required**: No  | 
| Team |  The name of the team that the player is assigned to in a match\. You define the team name in the matchmaking rule set\. **Type:** `String` **Required**: No  | 

## DescribePlayerSessionsRequest<a name="integration-server-sdk-go-dataypes-playersessions"></a>

An object that specifies which player sessions to retrieve\. The server process provides this information with a [DescribePlayerSessions\(\)](integration-server-sdk-go-actions.md#integration-server-sdk-go-describeplayersessions) call to GameLift\.


| Properties | **Description** | 
| --- | --- | 
| GameSessionID |  A unique game session identifier\. Use this parameter to request all player sessions for the specified game session\.  Game session ID format is `arn:aws:gamelift:<region>::gamesession/fleet-<fleet ID>/<ID string>`\. The `GameSessionID` is a custom ID string or a generated string\.  **Type:** `String` **Required**: No  | 
| PlayerSessionID |  The unique identifier for a player session\. Use this parameter to request a single specific player session\. **Type:** `String` **Required**: No  | 
| PlayerID |  The unique identifier for a player\. Use this parameter to request all player sessions for a specific player\. See [Generate player IDs](player-sessions-player-identifiers.md)\. **Type:** `String` **Required**: No  | 
| PlayerSessionStatusFilter |  The player session status to filter results on\. Possible player session statuses include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk-go-datatypes.html) **Type:** `String` **Required**: No  | 
| NextToken |  The token indicating the start of the next page of results\. To specify the start of the result set, don't provide a value\. If you provide a player session ID, this parameter is ignored\. **Type:** `String` **Required**: No  | 
| Limit |  The maximum number of results to return\. If you provide a player session ID, this parameter is ignored\. **Type:** `int` **Required**: No  | 

## StopMatchBackfillRequest<a name="integration-server-sdk-go-dataypes-stopmatchbackfillrequest"></a>

The data type used to cancel a matchmaking backfill request\. The information is communicated to the GameLift service in a [StopMatchBackfill\(\)](integration-server-sdk-go-actions.md#integration-server-sdk-go-stopmatchbackfill) call\.


| Properties | **Description** | 
| --- | --- | 
| GameSessionArn |  The unique game session identifier of the request being canceled\. **Type:** `string` **Required**: No  | 
| MatchmakingConfigurationArn |  The unique identifier of the matchmaker this request was sent to\. **Type:** `string` **Required**: No  | 
| TicketId |  The unique identifier of the backfill request ticket to be canceled\. **Type:** `string` **Required**: No  | 

## GetFleetRoleCredentialsRequest<a name="integration-server-sdk-go-dataypes-getfleetrolecredentialsrequest"></a>

The role credentials that extend limited access to your AWS resources to the game server\. For more information see, [Set up an IAM service role for GameLift](setting-up-role.md)\.


| Properties | **Description** | 
| --- | --- | 
| RoleArn | The ARN of the service role that extends limited access to your AWS resources\.**Type:** `string`**Required**: Yes | 
| RoleSessionName | The name of the session that describes the use of the role credentials\.**Type:** `string`**Required:** Yes | 