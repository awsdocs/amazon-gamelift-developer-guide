# Amazon GameLift Server API \(C\+\+\) Reference: Data Types<a name="integration-server-sdk-cpp-ref-datatypes"></a>

This Amazon GameLift C\+\+ Server API reference can help you prepare your multiplayer game for use with Amazon GameLift\. For details on the integration process, see [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)\.

This API is defined in `GameLiftServerAPI.h`, `LogParameters.h`, and `ProcessParameters.h`\.
+ [Actions](integration-server-sdk-cpp-ref-actions.md)
+ Data Types

## DescribePlayerSessionsRequest<a name="integration-server-sdk-cpp-ref-dataypes-playersessions"></a>

This data type is used to specify which player session\(s\) to retrieve\. You can use it as follows: 
+ Provide a PlayerSessionId to request a specific player session\.
+ Provide a GameSessionId to request all player sessions in the specified game session\.
+ Provide a PlayerId to request all player sessions for the specified player\.

For large collections of player sessions, use the pagination parameters to retrieve results in sequential blocks\.

### Contents<a name="integration-server-sdk-cpp-ref-dataypes-playersessions-contents"></a>

**GameSessionId**  
Unique game session identifier\. Use this parameter to request all player sessions for the specified game session\. Game session ID format is as follows: `arn:aws:gamelift:<region>::gamesession/fleet-<fleet ID>/<ID string>`\. The value of <ID string> is either a custom ID string or \(if one was specified when the game session was created\) a generated string\.   
Type: String  
Required: No

**Limit**  
Maximum number of results to return\. Use this parameter with *NextToken* to get results as a set of sequential pages\. If a player session ID is specified, this parameter is ignored\.  
Type: Integer  
Required: No

**NextToken**  
Token indicating the start of the next sequential page of results\. Use the token that is returned with a previous call to this action\. To specify the start of the result set, do not specify a value\. If a player session ID is specified, this parameter is ignored\.  
Type: String  
Required: No

**PlayerId**  
Unique identifier for a player\. Player IDs are defined by the developer\. See [Generate Player IDs](player-sessions-player-identifiers.md)\.  
Type: String  
Required: No

**PlayerSessionId**  
Unique identifier for a player session\.  
Type: String  
Required: No

**PlayerSessionStatusFilter**  
Player session status to filter results on\. Possible player session statuses include the following:  
+ RESERVED – The player session request has been received, but the player has not yet connected to the server process and/or been validated\.
+ ACTIVE – The player has been validated by the server process and is currently connected\.
+ COMPLETED – The player connection has been dropped\.
+ TIMEDOUT – A player session request was received, but the player did not connect and/or was not validated within the time\-out limit \(60 seconds\)\.
Type: String  
Required: No

## LogParameters<a name="integration-server-sdk-cpp-ref-dataypes-log"></a>

This data type is used to identify which files generated during a game session that you want Amazon GameLift to upload and store once the game session ends\. This information is communicated to the Amazon GameLift service in a [ProcessReady\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processready) call\.

### Contents<a name="integration-server-sdk-cpp-ref-dataypes-log-contents"></a>

**logPaths**  
Directory paths to game server log files that you want Amazon GameLift to store for future access\. These files are generated during each game session\. File paths and names are defined in your game server and stored in the root game build directory\. For example, if your game build stores game session logs in a path like `MyGame\sessionlogs\`, then the log path would be `c:\game\MyGame\sessionLogs` \(on a Windows instance\) or `/local/game/MyGame/sessionLogs` \(on a Linux instance\)\.   
Type: std:vector<std::string>  
Required: No

## ProcessParameters<a name="integration-server-sdk-cpp-ref-dataypes-process"></a>

This data type contains the set of parameters sent to the Amazon GameLift service in a [ProcessReady\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processready) call\.

### Contents<a name="integration-server-sdk-cpp-ref-dataypes-process-contents"></a>

**port**  
Port number the server process listens on for new player connections\. The value must fall into the port range configured for any fleet deploying this game server build\. This port number is included in game session and player session objects, which game sessions use when connecting to a server process\.   
Type: Integer   
Required: Yes

**logParameters**  
Object with a list of directory paths to game session log files\.   
Type: Aws::GameLift::Server::[LogParameters](#integration-server-sdk-cpp-ref-dataypes-log)  
Required: No

**onStartGameSession**  
Name of callback function that the Amazon GameLift service calls to activate a new game session\. Amazon GameLift calls this function in response to the client request [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html)\. The callback function passes a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object \(defined in the *Amazon GameLift Service API Reference*\)\.   
Type: `const std::function<void(Aws::GameLift::Model::GameSession)> onStartGameSession`   
Required: Yes

**onProcessTerminate**  
Name of callback function that the Amazon GameLift service calls to force the server process to shut down\. After calling this function, Amazon GameLift waits five minutes for the server process to shut down and respond with a [ProcessEnding\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processending) call\. If no response is receive, it shuts down the server process\.  
Type: `std::function<void()> onProcessTerminate`  
Required: No

**onHealthCheck**  
Name of callback function that the Amazon GameLift service calls to request a health status report from the server process\. Amazon GameLift calls this function every 60 seconds\. After calling this function Amazon GameLift waits 60 seconds for a response, and if none is received\. records the server process as unhealthy\.  
Type: `std::function<bool()> onHealthCheck`  
Required: No

**onUpdateGameSession**  
Name of callback function that the Amazon GameLift service calls to provide an updated game session object\. Amazon GameLift calls this function once a [ match backfill](match-backfill.md) request has been processed\. It passes a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object, a status update \(`updateReason`\), and the match backfill ticket ID\.   
Type: `std::function<void(Aws::GameLift::Server::Model::UpdateGameSession)> onUpdateGameSession`   
Required: No

## StartMatchBackfillRequest<a name="integration-server-sdk-cpp-ref-dataypes-startmatchbackfillrequest"></a>

This data type is used to send a matchmaking backfill request\. The information is communicated to the Amazon GameLift service in a [StartMatchBackfill\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-startmatchbackfill) call\.

### Contents<a name="integration-server-sdk-cpp-ref-dataypes-startbackfill-contents"></a>

**GameSessionArn**  
 Unique game session identifier\. The API action [GetGameSessionId\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-getgamesessionid) returns the identifier in ARN format\.  
Type: String  
Required: Yes

**MatchmakingConfigurationArn**  
Unique identifier, in the form of an ARN, for the matchmaker to use for this request\. To find the matchmaker that was used to create the original game session, look in the game session object, in the matchmaker data property\. Learn more about matchmaker data in [Work with Matchmaker Data](match-server.md#match-server-data)\.   
Type: String  
Required: Yes

**Players**  
A set of data representing all players who are currently in the game session\. The matchmaker uses this information to search for new players who are good matches for the current players\. See the *Amazon GameLift API Reference Guide* for a description of the Player object format\. To find player attributes, IDs, and team assignments, look in the game session object, in the matchmaker data property\. If latency is used by the matchmaker, gather updated latency for the current region and include it in each player's data\.   
Type: std:vector[<Player>](https://docs.aws.amazon.com/gamelift/latest/apireference/API_Player.html)  
Required: Yes

**TicketId**  
Unique identifier for a matchmaking or match backfill request ticket\. If no value is provided here, Amazon GameLift will generate one in the form of a UUID\. Use this identifier to track the match backfill ticket status or cancel the request if needed\.   
Type: String  
Required: No

## StopMatchBackfillRequest<a name="integration-server-sdk-cpp-ref-dataypes-stopmatchbackfillrequest"></a>

This data type is used to cancel a matchmaking backfill request\. The information is communicated to the Amazon GameLift service in a [StopMatchBackfill\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-stopmatchbackfill) call\.

### Contents<a name="integration-server-sdk-cpp-ref-dataypes-stopbackfill-contents"></a>

**GameSessionArn**  
Unique game session identifier associated with the request being canceled\.   
Type: String  
Required: Yes

**MatchmakingConfigurationArn**  
Unique identifier of the matchmaker this request was sent to\.   
Type: String  
Required: Yes

**TicketId**  
Unique identifier of the backfill request ticket to be canceled\.  
Type: String  
Required: Yes