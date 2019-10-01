# Amazon GameLift Server API \(C\#\) Reference: Actions<a name="integration-server-sdk-csharp-ref-actions"></a>

This Amazon GameLift C\# Server API reference can help you prepare your multiplayer game for use with Amazon GameLift\. For details on the integration process, see [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)\.

This API is defined in `GameLiftServerAPI.cs`, `LogParameters.cs`, and `ProcessParameters.cs`\.
+ Actions
+ [Data Types](integration-server-sdk-csharp-ref-datatypes.md)

## AcceptPlayerSession\(\)<a name="integration-server-sdk-csharp-ref-acceptplayersession"></a>

Notifies the Amazon GameLift service that a player with the specified player session ID has connected to the server process and needs validation\. Amazon GameLift verifies that the player session ID is valid—that is, that the player ID has reserved a player slot in the game session\. Once validated, Amazon GameLift changes the status of the player slot from RESERVED to ACTIVE\. 

### Syntax<a name="integration-server-sdk-csharp-ref-acceptplayersession-syntax"></a>

```
GenericOutcome AcceptPlayerSession(String playerSessionId)
```

### Parameters<a name="integration-server-sdk-csharp-ref-acceptplayersession-parameter"></a>

**playerSessionId**  
Unique ID issued by GameLift when a new player session is created\. A player session ID is specified in a `PlayerSession` object, which is returned in response to a client call to the *AWS SDK Amazon GameLift API* actions [ StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html), [ CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html), [ DescribeGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionPlacement.html), or [ DescribePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html)\.  
Type: String  
Required: No

### Return Value<a name="integration-server-sdk-csharp-ref-acceptplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\. 

### Example<a name="integration-server-sdk-csharp-ref-acceptplayersession-example"></a>

This example illustrates a function for handling a connection request, including validating and rejecting invalid player session IDs\. 

```
void ReceiveConnectingPlayerSessionID (Connection connection, String playerSessionId){
    var acceptPlayerSessionOutcome =  GameLiftServerAPI.AcceptPlayerSession(playerSessionId);
     if(acceptPlayerSessionOutcome.Success)
    {
        connectionToSessionMap.emplace(connection, playerSessionId);
        connection.Accept();
    }
     else 
    {
        connection.Reject(acceptPlayerSessionOutcome.Error.ErrorMessage);    }       
}
```

## ActivateGameSession\(\)<a name="integration-server-sdk-csharp-ref-activategamesession"></a>

Notifies the Amazon GameLift service that the server process has activated a game session and is now ready to receive player connections\. This action should be called as part of the `onStartGameSession()` callback function, after all game session initialization has been completed\.

### Syntax<a name="integration-server-sdk-csharp-ref-activategamesession-syntax"></a>

```
GenericOutcome ActivateGameSession()
```

### Parameters<a name="integration-server-sdk-csharp-ref-activategamesession-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-activategamesession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-csharp-ref-activategamesession-example"></a>

This example shows `ActivateGameSession()` being called as part of the `onStartGameSession()` delegate function\. 

```
void OnStartGameSession(GameSession gameSession)
{
    // game-specific tasks when starting a new game session, such as loading map   

    // When ready to receive players   
    var activateGameSessionOutcome = GameLiftServerAPI.ActivateGameSession();
}
```

## DescribePlayerSessions\(\)<a name="integration-server-sdk-csharp-ref-describeplayersessions"></a>

Retrieves player session data, including settings, session metadata, and player data\. Use this action to get information for a single player session, for all player sessions in a game session, or for all player sessions associated with a single player ID\.

### Syntax<a name="integration-server-sdk-csharp-ref-describeplayersessions-syntax"></a>

```
DescribePlayerSessionsOutcome DescribePlayerSessions(DescribePlayerSessionsRequest describePlayerSessionsRequest)
```

### Parameters<a name="integration-server-sdk-csharp-ref-describeplayersessions-parameter"></a>

**describePlayerSessionsRequest**  
A [DescribePlayerSessionsRequest](integration-server-sdk-csharp-ref-datatypes.md#integration-server-sdk-csharp-ref-dataypes-playersessions) object describing which player sessions to retrieve\.  
Required: Yes

### Return Value<a name="integration-server-sdk-csharp-ref-describeplayersessions-return"></a>

If successful, returns a `DescribePlayerSessionsOutcome` object containing a set of player session objects that fit the request parameters\. Player session objects have a structure identical to the AWS SDK Amazon GameLift API [PlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_PlayerSession.html) data type\.

### Example<a name="integration-server-sdk-csharp-ref-describeplayersessions-example"></a>

This example illustrates a request for all player sessions actively connected to a specified game session\. By omitting *NextToken* and setting the *Limit* value to 10, Amazon GameLift will return the first 10 player sessions records matching the request\.

```
// Set request parameters 
var describePlayerSessionsRequest = new Aws.GameLift.Server.Model.DescribePlayerSessionsRequest()
{
    GameSessionId = GameLiftServerAPI.GetGameSessionId().Result,    //gets the ID for the current game session
    Limit = 10,
    PlayerSessionStatusFilter = PlayerSessionStatusMapper.GetNameForPlayerSessionStatus(PlayerSessionStatus.ACTIVE)
}; 
// Call DescribePlayerSessions
Aws::GameLift::DescribePlayerSessionsOutcome playerSessionsOutcome = 
    Aws::GameLift::Server::Model::DescribePlayerSessions(describePlayerSessionRequest);
```

## GetGameSessionId\(\)<a name="integration-server-sdk-csharp-ref-getgamesessionid"></a>

Retrieves the ID of the game session currently being hosted by the server process, if the server process is active\. 

### Syntax<a name="integration-server-sdk-csharp-ref-getgamesessionid-syntax"></a>

```
AwsStringOutcome GetGameSessionId()
```

### Parameters<a name="integration-server-sdk-csharp-ref-getgamesessionid-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-getgamesessionid-return"></a>

If successful, returns the game session ID as an `AwsStringOutcome` object\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-csharp-ref-getgamesessionid-example"></a>

```
var getGameSessionIdOutcome = GameLiftServerAPI.GetGameSessionId();
```

## GetInstanceCertificate\(\)<a name="integration-server-sdk-csharp-ref-getinstancecertificate"></a>

Retrieves the file location of a pem\-encoded TLS certificate that is associated with the fleet and its instances\. This certificate is generated when a new fleet is created with the certificate configuration set to GENERATED\. Use this certificate to establish a secure connection with a game client and to encrypt client/server communication\. 

### Syntax<a name="integration-server-sdk-csharp-ref-getinstancecertificate-syntax"></a>

```
GetInstanceCertificateOutcome GetInstanceCertificate();
```

### Parameters<a name="integration-server-sdk-csharp-ref-getinstancecertificate-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-getinstancecertificate-return"></a>

If successful, returns a `GetInstanceCertificateOutcome` object containing the location of the fleet's TLS certificate file, which is stored on the instance\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-csharp-ref-getinstancecertificate-example"></a>

```
var getInstanceCertificateOutcome = GameLiftServerAPI.GetInstanceCertificate();
```

## GetSdkVersion\(\)<a name="integration-server-sdk-csharp-ref-getsdk"></a>

Returns the current version number of the SDK built into the server process\.

### Syntax<a name="integration-server-sdk-csharp-ref-getsdk-syntax"></a>

```
AwsStringOutcome GetSdkVersion()
```

### Parameters<a name="integration-server-sdk-csharp-ref-getsdk-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-getsdk-return"></a>

If successful, returns the current SDK version as an `AwsStringOutcome` object\. The returned string includes the version number only \(ex\. "3\.1\.5"\)\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-csharp-ref-getsdk-example"></a>

```
var getSdkVersionOutcome = GameLiftServerAPI.GetSdkVersion(); 
```

## GetTerminationTime\(\)<a name="integration-server-sdk-csharp-ref-getterm"></a>

Returns the time that a server process is scheduled to be shut down, if a termination time is available\. A server process takes this action after receiving an `onProcessTerminate()` callback from the Amazon GameLift service\. A server process may be shut down for several reasons: \(1\) process poor health, \(2\) when an instance is being terminated during a scale\-down event, or \(3\) when an instance is being terminated due to a [spot\-instance interruption](spot-tasks.md)\. 

If the process has received an `onProcessTerminate()` callback, the value returned is the estimated termination time in epoch seconds\. If no termination time is available, the value returned is \-1, which indicates that the server process may be terminated at any time\. If the process has not received an `onProcessTerminate()` callback, the returned value will always be \-1\. Learn more about [shutting down a server process](gamelift-sdk-server-api.md#gamelift-sdk-server-terminate)\.

### Syntax<a name="integration-server-sdk-csharp-ref-getterm-syntax"></a>

```
AwsLongOutcome GetTerminationTime()
```

### Parameters<a name="integration-server-sdk-csharp-ref-getterm-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-getterm-return"></a>

If successful, returns the current SDK version as an `AwsLongOutcome` object\. The value is either the termination time in epoch seconds, or the value \-1\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-csharp-ref-getterm-example"></a>

```
var getTerminationTimeOutcome = GameLiftServerAPI.GetTerminationTime(); 
```

## InitSDK\(\)<a name="integration-server-sdk-csharp-ref-initsdk"></a>

Initializes the Amazon GameLift SDK\. This method should be called on launch, before any other Amazon GameLift\-related initialization occurs\.

### Syntax<a name="integration-server-sdk-csharp-ref-initsdk-syntax"></a>

```
InitSDKOutcome InitSDK()
```

### Parameters<a name="integration-server-sdk-csharp-ref-initsdk-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-initsdk-return"></a>

If successful, returns an InitSdkOutcome object indicating that the server process is ready to call [ProcessReady\(\)](#integration-server-sdk-csharp-ref-processready)\. 

### Example<a name="integration-server-sdk-csharp-ref-initsdk-example"></a>

```
var initSDKOutcome = GameLiftServerAPI.InitSDK(); 
```

## ProcessEnding\(\)<a name="integration-server-sdk-csharp-ref-processending"></a>

Notifies the Amazon GameLift service that the server process is shutting down\. This method should exit with an exit code of 0; a non\-zero exit code results in an event message that the process did not exit cleanly\.

### Syntax<a name="integration-server-sdk-csharp-ref-processending-syntax"></a>

```
GenericOutcome ProcessEnding()
```

### Parameters<a name="integration-server-sdk-csharp-ref-processending-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-processending-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-csharp-ref-processending-example"></a>

```
var processEndingOutcome = GameLiftServerAPI.ProcessEnding();
```

## ProcessReady\(\)<a name="integration-server-sdk-csharp-ref-processready"></a>

Notifies the Amazon GameLift service that the server process is ready to host game sessions\. This method should be called after successfully invoking [InitSDK\(\)](#integration-server-sdk-csharp-ref-initsdk) and completing any setup tasks required before the server process can host a game session\.

### Syntax<a name="integration-server-sdk-csharp-ref-processready-syntax"></a>

```
GenericOutcome ProcessReady(ProcessParameters processParameters)
```

### Parameters<a name="integration-server-sdk-csharp-ref-processready-parameter"></a>

**processParameters**  
A [ProcessParameters](integration-server-sdk-csharp-ref-datatypes.md#integration-server-sdk-csharp-ref-dataypes-process) object communicating the following information about the server process:  
+ Names of callback methods, implemented in the game server code, that the Amazon GameLift service invokes to communicate with the server process\.
+ Port number that the server process is listening on\.
+ Path to any game session\-specific files that you want Amazon GameLift to capture and store\.
Required: Yes

### Return Value<a name="integration-server-sdk-csharp-ref-processready-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-csharp-ref-processready-example"></a>

This example illustrates both the [ProcessReady\(\)](#integration-server-sdk-csharp-ref-processready) call and delegate function implementations\.

```
// Set parameters and call ProcessReady
var processParams = new ProcessParameters(
   this.OnGameSession,
   this.OnProcessTerminate,
   this.OnHealthCheck,
   this.OnGameSessionUpdate,
   port,
   new LogParameters(new List<string>()          // Examples of log and error files written by the game server
   {
      "C:\\game\\logs",
      "C:\\game\\error"
   })
);

var processReadyOutcome = GameLiftServerAPI.ProcessReady(processParams);

// Implement callback functions
void OnGameSession(GameSession gameSession)
{
   // game-specific tasks when starting a new game session, such as loading map
   // When ready to receive players
   var activateGameSessionOutcome = GameLiftServerAPI.ActivateGameSession();
}

void OnProcessTerminate()
{
   // game-specific tasks required to gracefully shut down a game session, 
   // such as notifying players, preserving game state data, and other cleanup
    var ProcessEndingOutcome = GameLiftServerAPI.ProcessEnding();
    GameLiftServerAPI.Destroy();
}

bool OnHealthCheck()
{
    bool isHealthy;
    // complete health evaluation within 60 seconds and set health
    return isHealthy;
}
```

## RemovePlayerSession\(\)<a name="integration-server-sdk-csharp-ref-removeplayersession"></a>

Notifies the Amazon GameLift service that a player with the specified player session ID has disconnected from the server process\. In response, Amazon GameLift changes the player slot to available, which allows it to be assigned to a new player\. 

### Syntax<a name="integration-server-sdk-csharp-ref-removeplayersession-syntax"></a>

```
GenericOutcome RemovePlayerSession(String playerSessionId)
```

### Parameters<a name="integration-server-sdk-csharp-ref-removeplayersession-parameter"></a>

**playerSessionId**  
Unique ID issued by GameLift when a new player session is created\. A player session ID is specified in a `PlayerSession` object, which is returned in response to a client call to the *AWS SDK Amazon GameLift API* actions [ StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html), [ CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html), [ DescribeGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionPlacement.html), or [ DescribePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html)\.  
Type: String  
Required: No

### Return Value<a name="integration-server-sdk-csharp-ref-removeplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-csharp-ref-removeplayersession-example"></a>

```
Aws::GameLift::GenericOutcome disconnectOutcome = 
    Aws::GameLift::Server::RemovePlayerSession(playerSessionId);
```

## StartMatchBackfill\(\)<a name="integration-server-sdk-csharp-ref-startmatchbackfill"></a>

Sends a request to find new players for open slots in a game session created with FlexMatch\. See also the AWS SDK action [StartMatchBackfill\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchBackfill.html)\. With this action, match backfill requests can be initiated by a game server process that is hosting the game session\. Learn more about the FlexMatch backfill feature in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

This action is asynchronous\. If new players are successfully matched, the Amazon GameLift service delivers updated matchmaker data using the callback function `OnUpdateGameSession()`\.

A server process can have only one active match backfill request at a time\. To send a new request, first call [StopMatchBackfill\(\)](#integration-server-sdk-csharp-ref-stopmatchbackfill) to cancel the original request\.

### Syntax<a name="integration-server-sdk-csharp-ref-startmatchbackfill-syntax"></a>

```
StartMatchBackfillOutcome StartMatchBackfill (StartMatchBackfillRequest startBackfillRequest);
```

### Parameters<a name="integration-server-sdk-csharp-ref-startmatchbackfill-parameter"></a>

**StartMatchBackfillRequest**  
A [StartMatchBackfillRequest](integration-server-sdk-csharp-ref-datatypes.md#integration-server-sdk-csharp-ref-dataypes-startmatchbackfillrequest) object that communicates the following information:  
+ A ticket ID to assign to the backfill request\. This information is optional; if no ID is provided, Amazon GameLift will autogenerate one\.
+ The matchmaker to send the request to\. The full configuration ARN is required\. This value can be acquired from the game session's matchmaker data\.
+ The ID of the game session that is being backfilled\.
+ Available matchmaking data for the game session's current players\.
Required: Yes

### Return Value<a name="integration-server-sdk-csharp-ref-startmatchbackfill-return"></a>

Returns a StartMatchBackfillOutcome object with the match backfill ticket ID or failure with an error message\. 

### Example<a name="integration-server-sdk-csharp-ref-startmatchbackfill-example"></a>

```
// Build a backfill request
var startBackfillRequest = new AWS.GameLift.Server.Model.StartMatchBackfillRequest()
{
    TicketId = "a ticket ID", //optional
    MatchmakingConfigurationArn = "the matchmaker configuration ARN", 
    GameSessionId = GameLiftServerAPI.GetGameSessionId().Result,    // gets ID for current game session
        //get player data for all currently connected players
            MatchmakerData matchmakerData =        
              MatchmakerData.FromJson(gameSession.MatchmakerData);  // gets matchmaker data for current players
            // get matchmakerData.Players
            // remove data for players who are no longer connected
    Players = ListOfPlayersRemainingInTheGame
};

// Send backfill request
var startBackfillOutcome = GameLiftServerAPI.StartMatchBackfill(startBackfillRequest);

// Implement callback function for backfill
void OnUpdateGameSession(GameSession myGameSession)
{
   // game-specific tasks to prepare for the newly matched players and update matchmaker data as needed  
}
```

## StopMatchBackfill\(\)<a name="integration-server-sdk-csharp-ref-stopmatchbackfill"></a>

Cancels an active match backfill request that was created with [StartMatchBackfill\(\)](#integration-server-sdk-csharp-ref-startmatchbackfill)\. See also the AWS SDK action [StopMatchmaking\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html)\. Learn more about the FlexMatch backfill feature in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

### Syntax<a name="integration-server-sdk-csharp-ref-stopmatchbackfill-syntax"></a>

```
GenericOutcome StopMatchBackfill (StopMatchBackfillRequest stopBackfillRequest);
```

### Parameters<a name="integration-server-sdk-csharp-ref-stopmatchbackfill-parameter"></a>

**StopMatchBackfillRequest**  
A [StopMatchBackfillRequest](integration-server-sdk-csharp-ref-datatypes.md#integration-server-sdk-csharp-ref-dataypes-stopmatchbackfillrequest) object identifying the matchmaking ticket to cancel:   
+ ticket ID assigned to the backfill request being canceled
+ matchmaker the backfill request was sent to
+ game session associated with the backfill request
Required: Yes

### Return Value<a name="integration-server-sdk-csharp-ref-stopmatchbackfill-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-csharp-ref-stopmatchbackfill-example"></a>

```
// Set backfill stop request parameters

var stopBackfillRequest = new AWS.GameLift.Server.Model.StopMatchBackfillRequest()
{
    TicketId = "a ticket ID", //optional, if not provided one is autogenerated
    MatchmakingConfigurationArn = "the matchmaker configuration ARN", //from the game session matchmaker data
    GameSessionId = GameLiftServerAPI.GetGameSessionId().Result    //gets the ID for the current game session
};

var stopBackfillOutcome = GameLiftServerAPI.StopMatchBackfillRequest(stopBackfillRequest);
```

## TerminateGameSession\(\)<a name="integration-server-sdk-csharp-ref-terminategamesession"></a>

Notifies the Amazon GameLift service that the server process has shut down the game session\. \(Currently, each server process hosts only one game session at a time, so there's no need to specify which session\.\) This action should be called at the end of the game session shutdown process\. After calling this action, the server process can call [ProcessReady\(\)](#integration-server-sdk-csharp-ref-processready) to signal its availability to host a new game session\. Alternatively, it can call [ProcessEnding\(\)](#integration-server-sdk-csharp-ref-processending) to shut down the server process and terminate the instance\.

### Syntax<a name="integration-server-sdk-csharp-ref-terminategamesession-syntax"></a>

```
GenericOutcome TerminateGameSession()
```

### Parameters<a name="integration-server-sdk-csharp-ref-terminategamesession-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-csharp-ref-terminategamesession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-csharp-ref-terminategamesession-example"></a>

This example illustrates a server process at the end of a game session\.

```
// game-specific tasks required to gracefully shut down a game session, 
// such as notifying players, preserving game state data, and other cleanup

var terminateGameSessionOutcome = GameLiftServerAPI.TerminateGameSession();
var processReadyOutcome = GameLiftServerAPI.ProcessReady(processParams);
```

## UpdatePlayerSessionCreationPolicy\(\)<a name="integration-server-sdk-csharp-ref-updateplayersessioncreationpolicy"></a>

Updates the current game session's ability to accept new player sessions\. A game session can be set to either accept or deny all new player sessions\. \(See also the [UpdateGameSession\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html) action in the *Amazon GameLift Service API Reference*\)\.

### Syntax<a name="integration-server-sdk-csharp-ref-updateplayersessioncreationpolicy-syntax"></a>

```
GenericOutcome UpdatePlayerSessionCreationPolicy(PlayerSessionCreationPolicy playerSessionPolicy)
```

### Parameters<a name="integration-server-sdk-csharp-ref-updateplayersessioncreationpolicy-parameter"></a>

**newPlayerSessionPolicy**  
String value indicating whether the game session accepts new players\.   
Type: [PlayerSessionCreationPolicy](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift_1_1_model.html#afa8a7527defe9e7ca0caebc239182c53) enum\. Valid values include:  
+ **ACCEPT\_ALL** – Accept all new player sessions\.
+ **DENY\_ALL** – Deny all new player sessions\.
Required: Yes

### Return Value<a name="integration-server-sdk-csharp-ref-updateplayersessioncreationpolicy-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-csharp-ref-updateplayersessioncreationpolicy-example"></a>

This example sets the current game session's join policy to accept all players\.

```
var updatePlayerSessionCreationPolicyOutcomex = 
    GameLiftServerAPI.UpdatePlayerSessionCreationPolicy(PlayerSessionCreationPolicy.ACCEPT_ALL);
```