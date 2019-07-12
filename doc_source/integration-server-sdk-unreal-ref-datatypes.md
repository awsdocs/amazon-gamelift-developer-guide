# Amazon GameLift Server API Reference for Unreal Engine: Data Types<a name="integration-server-sdk-unreal-ref-datatypes"></a>

This Amazon GameLift Server API reference can help you prepare your Unreal Engine game projects for use with Amazon GameLift\. For details on the integration process, see [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)\.

This API is defined in `GameLiftServerSDK.h` and `GameLiftServerSDKModels.h`\.

To set up the Unreal Engine plugin and see code examples [Add Amazon GameLift to an Unreal Engine Game Server Project](integration-engines-setup-unreal.md)\.
+ [Actions](integration-server-sdk-unreal-ref-actions.md)
+ Data Types

## FProcessParameters<a name="integration-server-sdk-unreal-ref-dataypes-process"></a>

This data type contains the set of parameters sent to the Amazon GameLift service in a [ProcessReady\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-processready) call\.

### Contents<a name="integration-server-sdk-unreal-ref-dataypes-process-contents"></a>

**port**  
Port number the server process will listen on for new player connections\. The value must fall into the port range configured for any fleet deploying this game server build\. This port number is included in game session and player session objects, which game sessions use when connecting to a server process\.   
Type: Integer   
Required: Yes

**logParameters**  
Object with a list of directory paths to game session log files\.   
Type: TArray<FString>  
Required: No

**onStartGameSession**  
Name of callback function that the Amazon GameLift service calls to activate a new game session\. Amazon GameLift calls this function in response to the client request [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html)\. The callback function takes a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object \(defined in the *Amazon GameLift Service API Reference*\)\.   
Type: FOnStartGameSession   
Required: Yes

**onProcessTerminate**  
Name of callback function that the Amazon GameLift service calls to force the server process to shut down\. After calling this function, Amazon GameLift waits five minutes for the server process to shut down and respond with a [ProcessEnding\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-processending) call before it shuts down the server process\.  
Type: FSimpleDelegate  
Required: No

**onHealthCheck**  
Name of callback function that the Amazon GameLift service calls to request a health status report from the server process\. Amazon GameLift calls this function every 60 seconds\. After calling this function Amazon GameLift waits 60 seconds for a response, and if none is received\. records the server process as unhealthy\.  
Type: FOnHealthCheck  
Required: No

## FStartMatchBackfillRequest<a name="integration-server-sdk-unreal-ref-dataypes-startmatchbackfillrequest"></a>

This data type is used to send a matchmaking backfill request\. The information is communicated to the Amazon GameLift service in a [StartMatchBackfill\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-startmatchbackfill) call\.

### Contents<a name="integration-server-sdk-unreal-ref-dataypes-startbackfill-contents"></a>

**GameSessionArn**  
 Unique game session identifier\. The API action [GetGameSessionId\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-getgamesessionid) returns the identifier in ARN format\.  
Type: FString  
Required: Yes

**MatchmakingConfigurationArn**  
Unique identifier, in the form of an ARN, for the matchmaker to use for this request\. To find the matchmaker that was used to create the original game session, look in the game session object, in the matchmaker data property\. Learn more about matchmaker data in [Work with Matchmaker Data](match-server.md#match-server-data)\.   
Type: FString  
Required: Yes

**Players**  
A set of data representing all players who are currently in the game session\. The matchmaker uses this information to search for new players who are good matches for the current players\. See the *Amazon GameLift API Reference Guide* for a description of the Player object format\. To find player attributes, IDs, and team assignments, look in the game session object, in the matchmaker data property\. If latency is used by the matchmaker, gather updated latency for the current region and include it in each player's data\.   
Type: TArray[<FPlayer>](https://docs.aws.amazon.com/gamelift/latest/apireference/API_Player.html)  
Required: Yes

**TicketId**  
Unique identifier for a matchmaking or match backfill request ticket\. If no value is provided here, Amazon GameLift will generate one in the form of a UUID\. Use this identifier to track the match backfill ticket status or cancel the request if needed\.   
Type: FString  
Required: No

## FStopMatchBackfillRequest<a name="integration-server-sdk-unreal-ref-dataypes-stopmatchbackfillrequest"></a>

This data type is used to cancel a matchmaking backfill request\. The information is communicated to the Amazon GameLift service in a [StopMatchBackfill\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-stopmatchbackfill) call\.

### Contents<a name="integration-server-sdk-unreal-ref-dataypes-stopbackfill-contents"></a>

**GameSessionArn**  
Unique game session identifier associated with the request being canceled\.   
Type: FString  
Required: Yes

**MatchmakingConfigurationArn**  
Unique identifier of the matchmaker this request was sent to\.   
Type: FString  
Required: Yes

**TicketId**  
Unique identifier of the backfill request ticket to be canceled\.  
Type: FString  
Required: Yes