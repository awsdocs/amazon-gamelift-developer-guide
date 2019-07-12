# Amazon GameLift Server API Reference for Unreal Engine: Actions<a name="integration-server-sdk-unreal-ref-actions"></a>

This Amazon GameLift Server API reference can help you prepare your Unreal Engine game projects for use with Amazon GameLift\. For details on the integration process, see [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)\.

This API is defined in `GameLiftServerSDK.h` and `GameLiftServerSDKModels.h`\.

To set up the Unreal Engine plugin and see code examples [Add Amazon GameLift to an Unreal Engine Game Server Project](integration-engines-setup-unreal.md)\.
+ Actions
+ [Data Types](integration-server-sdk-unreal-ref-datatypes.md)

## AcceptPlayerSession\(\)<a name="integration-server-sdk-unreal-ref-acceptplayersession"></a>

Notifies the Amazon GameLift service that a player with the specified player session ID has connected to the server process and needs validation\. Amazon GameLift verifies that the player session ID is valid—that is, that the player ID has reserved a player slot in the game session\. Once validated, Amazon GameLift changes the status of the player slot from RESERVED to ACTIVE\. 

### Syntax<a name="integration-server-sdk-unreal-ref-acceptplayersession-syntax"></a>

```
FGameLiftGenericOutcome AcceptPlayerSession(const FString& playerSessionId)
```

### Parameters<a name="integration-server-sdk-unreal-ref-acceptplayersession-parameter"></a>

**playerSessionId**  
Unique ID issued by the Amazon GameLift service in response to a call to the AWS SDK Amazon GameLift API action [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html)\. The game client references this ID when connecting to the server process\.  
Type: FString  
Required: No

### Return Value<a name="integration-server-sdk-unreal-ref-acceptplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\. 

## ActivateGameSession\(\)<a name="integration-server-sdk-unreal-ref-activategamesession"></a>

Notifies the Amazon GameLift service that the server process has activated a game session and is now ready to receive player connections\. This action should be called as part of the `onStartGameSession()` callback function, after all game session initialization has been completed\.

### Syntax<a name="integration-server-sdk-unreal-ref-activategamesession-syntax"></a>

```
FGameLiftGenericOutcome ActivateGameSession()
```

### Parameters<a name="integration-server-sdk-unreal-ref-activategamesession-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-unreal-ref-activategamesession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

## GetGameSessionId\(\)<a name="integration-server-sdk-unreal-ref-getgamesessionid"></a>

Retrieves the ID of the game session currently being hosted by the server process, if the server process is active\. 

### Syntax<a name="integration-server-sdk-unreal-ref-getgamesessionid-syntax"></a>

```
FGameLiftStringOutcome GetGameSessionId()
```

### Parameters<a name="integration-server-sdk-unreal-ref-getgamesessionid-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-unreal-ref-getgamesessionid-return"></a>

If successful, returns the game session ID as an `FGameLiftStringOutcome` object\. If not successful, returns an error message\.

## GetSdkVersion\(\)<a name="integration-server-sdk-unreal-ref-getsdk"></a>

Returns the current version number of the SDK built into the server process\.

### Syntax<a name="integration-server-sdk-unreal-ref-getsdk-syntax"></a>

```
FGameLiftStringOutcome GetSdkVersion();
```

### Parameters<a name="integration-server-sdk-unreal-ref-getsdk-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-unreal-ref-getsdk-return"></a>

If successful, returns the current SDK version as an `FGameLiftStringOutcome` object\. The returned string includes the version number only \(ex\. "3\.1\.5"\)\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-unreal-ref-getsdk-example"></a>

```
Aws::GameLift::AwsStringOutcome SdkVersionOutcome = 
    Aws::GameLift::Server::GetSdkVersion();
```

## InitSDK\(\)<a name="integration-server-sdk-unreal-ref-initsdk"></a>

Initializes the Amazon GameLift SDK\. This method should be called on launch, before any other Amazon GameLift\-related initialization occurs\.

### Syntax<a name="integration-server-sdk-unreal-ref-initsdk-syntax"></a>

```
FGameLiftGenericOutcome InitSDK()
```

### Parameters<a name="integration-server-sdk-unreal-ref-initsdk-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-unreal-ref-initsdk-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

## ProcessEnding\(\)<a name="integration-server-sdk-unreal-ref-processending"></a>

Notifies the Amazon GameLift service that the server process is shutting down\. This method should be called after all other cleanup tasks, including shutting down all active game sessions\. This method should exit with an exit code of 0; a non\-zero exit code results in an event message that the process did not exit cleanly\.

### Syntax<a name="integration-server-sdk-unreal-ref-processending-syntax"></a>

```
FGameLiftGenericOutcome ProcessEnding()
```

### Parameters<a name="integration-server-sdk-unreal-ref-processending-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-unreal-ref-processending-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

## ProcessReady\(\)<a name="integration-server-sdk-unreal-ref-processready"></a>

Notifies the Amazon GameLift service that the server process is ready to host game sessions\. This method should be called after successfully invoking [InitSDK\(\)](#integration-server-sdk-unreal-ref-initsdk) and completing any setup tasks required before the server process can host a game session\.

### Syntax<a name="integration-server-sdk-unreal-ref-processready-syntax"></a>

```
FGameLiftGenericOutcome ProcessReady(FProcessParameters &processParameters)
```

### Parameters<a name="integration-server-sdk-unreal-ref-processready-parameter"></a>

**FProcessParameters**  
A [FProcessParameters](integration-server-sdk-unreal-ref-datatypes.md#integration-server-sdk-unreal-ref-dataypes-process) object communicating the following information about the server process:  
+ Names of callback methods, implemented in the game server code, that the Amazon GameLift service invokes to communicate with the server process\.
+ Port number that the server process is listening on\.
+ Path to any game session\-specific files that you want Amazon GameLift to capture and store\.
Required: Yes

### Return Value<a name="integration-server-sdk-unreal-ref-processready-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-unreal-ref-processready-example"></a>

See the sample code in [Using the Unreal Engine Plugin](integration-engines-setup-unreal.md#integration-engines-setup-unreal-code)\.

## RemovePlayerSession\(\)<a name="integration-server-sdk-unreal-ref-removeplayersession"></a>

Notifies the Amazon GameLift service that a player with the specified player session ID has disconnected from the server process\. In response, Amazon GameLift changes the player slot to available, which allows it to be assigned to a new player\. 

### Syntax<a name="integration-server-sdk-unreal-ref-removeplayersession-syntax"></a>

```
FGameLiftGenericOutcome RemovePlayerSession(const FString& playerSessionId)
```

### Parameters<a name="integration-server-sdk-unreal-ref-removeplayersession-parameter"></a>

**playerSessionId**  
Unique ID issued by the Amazon GameLift service in response to a call to the AWS SDK Amazon GameLift API action [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html)\. The game client references this ID when connecting to the server process\.  
Type: FString  
Required: No

### Return Value<a name="integration-server-sdk-unreal-ref-removeplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

## StartMatchBackfill\(\)<a name="integration-server-sdk-unreal-ref-startmatchbackfill"></a>

Sends a request to find new players for open slots in a game session created with FlexMatch\. See also the AWS SDK action [StartMatchBackfill\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchBackfill.html)\. With this action, match backfill requests can be initiated by a game server process that is hosting the game session\. Learn more about the FlexMatch backfill feature in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

A server process can have only one active match backfill request at a time\. To send a new request, first call [StopMatchBackfill\(\)](#integration-server-sdk-unreal-ref-stopmatchbackfill) to cancel the original request\.

### Syntax<a name="integration-server-sdk-unreal-ref-startmatchbackfill-syntax"></a>

```
FGameLiftStringOutcome StartMatchBackfill (FStartMatchBackfillRequest &startBackfillRequest);
```

### Parameters<a name="integration-server-sdk-unreal-ref-startmatchbackfill-parameter"></a>

**FStartMatchBackfillRequest**  
A [FStartMatchBackfillRequest](integration-server-sdk-unreal-ref-datatypes.md#integration-server-sdk-unreal-ref-dataypes-startmatchbackfillrequest) object that communicates the following information:  
+ A ticket ID to assign to the backfill request\. This information is optional; if no ID is provided, Amazon GameLift will autogenerate one\.
+ The matchmaker to send the request to\. The full configuration ARN is required\. This value can be acquired from the game session's matchmaker data\.
+ The ID of the game session that is being backfilled\.
+ Available matchmaking data for the game session's current players\.
Required: Yes

### Return Value<a name="integration-server-sdk-unreal-ref-startmatchbackfill-return"></a>

If successful, returns the match backfill ticket as a `FGameLiftStringOutcome` object\. If not successful, returns an error message\. Ticket status can be tracked using the AWS SDK action [DescribeMatchmaking\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html)\.

## StopMatchBackfill\(\)<a name="integration-server-sdk-unreal-ref-stopmatchbackfill"></a>

Cancels an active match backfill request that was created with [StartMatchBackfill\(\)](#integration-server-sdk-unreal-ref-startmatchbackfill)\. See also the AWS SDK action [StopMatchmaking\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html)\. Learn more about the FlexMatch backfill feature in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

### Syntax<a name="integration-server-sdk-unreal-ref-stopmatchbackfill-syntax"></a>

```
FGameLiftGenericOutcome StopMatchBackfill (FStopMatchBackfillRequest &stopBackfillRequest);
```

### Parameters<a name="integration-server-sdk-unreal-ref-stopmatchbackfill-parameter"></a>

**StopMatchBackfillRequest**  
A [FStopMatchBackfillRequest](integration-server-sdk-unreal-ref-datatypes.md#integration-server-sdk-unreal-ref-dataypes-stopmatchbackfillrequest) object identifying the matchmaking ticket to cancel:   
+ ticket ID assigned to the backfill request being canceled
+ matchmaker the backfill request was sent to
+ game session associated with the backfill request
Required: Yes

### Return Value<a name="integration-server-sdk-unreal-ref-stopmatchbackfill-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### <a name="integration-server-sdk-unreal-ref-stopmatchbackfill-example"></a>

## TerminateGameSession\(\)<a name="integration-server-sdk-unreal-ref-terminategamesession"></a>

Notifies the Amazon GameLift service that the server process has shut down the game session\. \(Currently, each server process hosts only one game session at a time, so there's no need to specify which session\.\) This action should be called at the end of the game session shutdown process\. After calling this action, the server process can call [ProcessReady\(\)](#integration-server-sdk-unreal-ref-processready) to signal its availability to host a new game session\. Alternatively, it can call [ProcessEnding\(\)](#integration-server-sdk-unreal-ref-processending) to shut down the server process and terminate the instance\.

### Syntax<a name="integration-server-sdk-unreal-ref-terminategamesession-syntax"></a>

```
FGameLiftGenericOutcome TerminateGameSession()
```

### Parameters<a name="integration-server-sdk-unreal-ref-terminategamesession-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-unreal-ref-terminategamesession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

## UpdatePlayerSessionCreationPolicy\(\)<a name="integration-server-sdk-unreal-ref-updateplayersessioncreationpolicy"></a>

Updates the current game session's ability to accept new player sessions\. A game session can be set to either accept or deny all new player sessions\. \(See also the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html) action in the *Amazon GameLift Service API Reference*\)\.

### Syntax<a name="integration-server-sdk-unreal-ref-updateplayersessioncreationpolicy-syntax"></a>

```
FGameLiftGenericOutcome UpdatePlayerSessionCreationPolicy(EPlayerSessionCreationPolicy policy)
```

### Parameters<a name="integration-server-sdk-unreal-ref-updateplayersessioncreationpolicy-parameter"></a>

**Policy**  
Value indicating whether the game session accepts new players\.   
Type: `EPlayerSessionCreationPolicy` enum\. Valid values include:  
+ **ACCEPT\_ALL** – Accept all new player sessions\.
+ **DENY\_ALL** – Deny all new player sessions\.
Required: Yes

### Return Value<a name="integration-server-sdk-unreal-ref-updateplayersessioncreationpolicy-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.