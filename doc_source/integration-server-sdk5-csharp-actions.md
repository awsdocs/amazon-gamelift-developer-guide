# GameLift server API \(C\#\) reference: Actions<a name="integration-server-sdk5-csharp-actions"></a>

This Amazon GameLift C\# Server API reference helps you prepare your multiplayer game for use with GameLift\. For details about the integration process, see [Add GameLift to your game server](gamelift-sdk-server-api.md)\.

**Topics**
+ [GetSdkVersion\(\)](#integration-server-sdk5-csharp-getsdkversion)
+ [InitSDK\(\)](#integration-server-sdk5-csharp-initsdk)
+ [ProcessReady\(\)](#integration-server-sdk5-csharp-processready)
+ [ProcessEnding\(\)](#integration-server-sdk5-csharp-processending)
+ [ActivateGameSession\(\)](#integration-server-sdk5-csharp-activategamesession)
+ [UpdatePlayerSessionCreationPolicy\(\)](#integration-server-sdk5-csharp-updateplayersessioncreationpolicy)
+ [GetGameSessionId\(\)](#integration-server-sdk5-csharp-getgamesessionid)
+ [GetTerminationTime\(\)](#integration-server-sdk5-csharp-getterm)
+ [AcceptPlayerSession\(\)](#integration-server-sdk5-csharp-acceptplayersession)
+ [RemovePlayerSession\(\)](#integration-server-sdk5-csharp-removeplayersession)
+ [DescribePlayerSessions\(\)](#integration-server-sdk5-csharp-describeplayersessions)
+ [StartMatchBackfill\(\)](#integration-server-sdk5-csharp-startmatchbackfill)
+ [StopMatchBackfill\(\)](#integration-server-sdk5-csharp-stopmatchbackfill)
+ [GetComputeCertificate\(\)](#integration-server-sdk5-csharp-getcomputertificate)
+ [GetFleetRoleCredentials\(\)](#integration-server-sdk5-csharp-getfleetrolecredentials)
+ [Destroy\(\)](#integration-server-sdk5-csharp-destroy)

## GetSdkVersion\(\)<a name="integration-server-sdk5-csharp-getsdkversion"></a>

Returns the current version number of the SDK built into the server process\.

### Syntax<a name="integration-server-sdk5-csharp-getsdkversion-syntax"></a>

```
AwsStringOutcome GetSdkVersion();
```

### Return value<a name="integration-server-sdk5-csharp-getsdkversion-return"></a>

If successful, returns the current SDK version as an AwsStringOutcome object\. The returned string includes the version number \(example `5.0.0`\)\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk5-csharp-getsdkversion-example"></a>

```
var getSdkVersionOutcome = GameLiftServerAPI.GetSdkVersion();
```

## InitSDK\(\)<a name="integration-server-sdk5-csharp-initsdk"></a>

Initializes the GameLift SDK\. Call this method on launch before any other initialization related to GameLift occurs\.

### Syntax<a name="integration-server-sdk5-csharp-initsdk-syntax"></a>

```
GenericOutcome InitSDK(ServerParameters serverParameters);
```

### Parameters<a name="integration-server-sdk5-csharp-initsdk-parameter"></a>

[ServerParameters](integration-server-sdk5-csharp-datatypes.md#integration-server-sdk5-csharp-dataypes-serverparameters)  
A `ServerParameters` object holds information about the server communication with GameLift from a GameLift Anywhere server\. This parameter isn't required for servers hosted on GameLift managed EC2 instances\.

### Return value<a name="integration-server-sdk5-csharp-initsdk-return"></a>

If successful, returns an InitSdkOutcome object to indicate that the server process is ready to call [ProcessReady\(\)](#integration-server-sdk5-csharp-processready)\. 

### Example<a name="integration-server-sdk5-csharp-initsdk-example"></a>

```
ServerParameters serverParameters = new ServerParameters(
    webSocketUrl,
    processId,
    hostId,
    fleetId);

//InitSDK will establish a local connection with GameLift's agent to enable further communication.
var initSDKOutcome = GameLiftServerAPI.InitSDK(serverParameters);
```

## ProcessReady\(\)<a name="integration-server-sdk5-csharp-processready"></a>

Notifies GameLift that the server process is ready to host game sessions\. Call this method after invoking [InitSDK\(\)](#integration-server-sdk5-csharp-initsdk)\. This method should be called only once per process\.

### Syntax<a name="integration-server-sdk5-csharp-processready-syntax"></a>

```
GenericOutcome ProcessReady(ProcessParameters processParameters)
```

### Parameters<a name="integration-server-sdk5-csharp-processready-parameter"></a>

**[ProcessParameters](integration-server-sdk5-csharp-datatypes.md#integration-server-sdk5-csharp-dataypes-process)**  
A `ProcessParameters` object holds information about the server process\.

### Return value<a name="integration-server-sdk5-csharp-processready-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk5-csharp-processready-example"></a>

This example illustrates both the method and delegate function implementations\.

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
```

## ProcessEnding\(\)<a name="integration-server-sdk5-csharp-processending"></a>

Notifies GameLift that the server process is shutting down\. This method should be called after all other cleanup tasks, including shutting down all active game sessions\. This method should exit with an exit code of 0; a non\-zero exit code results in an event message that the process did not exit cleanly\.

After the method exits with a code of 0, you can terminate the process with a successful exit code\. You can also exit the process with an error code\. If you exit with an error code, the fleet event will indicate that the process terminated abnormally \(`SERVER_PROCESS_TERMINATED_UNHEALTHY`\)\. 

### Syntax<a name="integration-server-sdk5-csharp-processending-syntax"></a>

```
GenericOutcome ProcessEnding()
```

### Return value<a name="integration-server-sdk5-csharp-processending-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk5-csharp-processending-example"></a>

```
var processEndingOutcome = GameLiftServerAPI.ProcessEnding();
if (processReadyOutcome.Success)
   Environment.Exit(0);
// otherwise, exit with error code
Environment.Exit(errorCode);
```

## ActivateGameSession\(\)<a name="integration-server-sdk5-csharp-activategamesession"></a>

Notifies GameLift that the server process has activated a game session and is now ready to receive player connections\. This action should be called as part of the `onStartGameSession()` callback function, after all game session initialization\.

### Syntax<a name="integration-server-sdk5-csharp-activategamesession-syntax"></a>

```
GenericOutcome ActivateGameSession()
```

### Return value<a name="integration-server-sdk5-csharp-activategamesession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk5-csharp-activategamesession-example"></a>

This example shows `ActivateGameSession()` being called as part of the `onStartGameSession()` delegate function\. 

```
void OnStartGameSession(GameSession gameSession)
{
    // game-specific tasks when starting a new game session, such as loading map   

    // When ready to receive players   
    var activateGameSessionOutcome = GameLiftServerAPI.ActivateGameSession();
}
```

## UpdatePlayerSessionCreationPolicy\(\)<a name="integration-server-sdk5-csharp-updateplayersessioncreationpolicy"></a>

Updates the current game session's ability to accept new player sessions\. A game session can be set to either accept or deny all new player sessions\.

### Syntax<a name="integration-server-sdk5-csharp-updateplayersessioncreationpolicy-syntax"></a>

```
GenericOutcome UpdatePlayerSessionCreationPolicy(PlayerSessionCreationPolicy playerSessionPolicy)
```

### Parameters<a name="integration-server-sdk5-csharp-updateplayersessioncreationpolicy-parameter"></a>

**playerSessionPolicy**  
String value that indicates whether the game session accepts new players\.   
Valid values include:  
+ **ACCEPT\_ALL** – Accept all new player sessions\.
+ **DENY\_ALL** – Deny all new player sessions\.

### Return value<a name="integration-server-sdk5-csharp-updateplayersessioncreationpolicy-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk5-csharp-updateplayersessioncreationpolicy-example"></a>

This example sets the current game session's join policy to accept all players\.

```
var updatePlayerSessionPolicyOutcome = GameLiftServerAPI.UpdatePlayerSessionCreationPolicy(PlayerSessionCreationPolicy.ACCEPT_ALL);
```

## GetGameSessionId\(\)<a name="integration-server-sdk5-csharp-getgamesessionid"></a>

Retrieves the ID of the game session hosted by the active server process\.

For idle processes that aren't activated with a game session, the call returns a `GameLiftError`\.

### Syntax<a name="integration-server-sdk5-csharp-getgamesessionid-syntax"></a>

```
AwsStringOutcome GetGameSessionId()
```

### Return value<a name="integration-server-sdk5-csharp-getgamesessionid-return"></a>

If successful, returns the game session ID as an `AwsStringOutcome` object\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk5-csharp-getgamesessionid-example"></a>

```
var getGameSessionIdOutcome = GameLiftServerAPI.GetGameSessionId();
```

## GetTerminationTime\(\)<a name="integration-server-sdk5-csharp-getterm"></a>

Returns the time that a server process is scheduled to be shut down, if a termination time is available\. A server process takes this action after receiving an `onProcessTerminate()` callback from GameLift\. GameLift calls `onProcessTerminate()` for the following reasons: 
+ When the server process has reported poor health or has not responded to GameLift\.
+ When terminating the instance during a scale\-down event\.
+ When an instance is terminated due to a [spot\-instance interruption](spot-tasks.md)\.

### Syntax<a name="integration-server-sdk5-csharp-getterm-syntax"></a>

```
AwsDateTimeOutcome GetTerminationTime()
```

### Return value<a name="integration-server-sdk5-csharp-getterm-return"></a>

If successful, returns the termination time as an `AwsDateTimeOutcome` object\. The value is the termination time, expressed in elapsed ticks since `0001 00:00:00`\. For example, the date time value `2020-09-13 12:26:40 -000Z` is equal to `637355968000000000` ticks\. If no termination time is available, returns an error message\.

### Example<a name="integration-server-sdk5-csharp-getterm-example"></a>

```
var getTerminationTimeOutcome = GameLiftServerAPI.GetTerminationTime(); 
```

## AcceptPlayerSession\(\)<a name="integration-server-sdk5-csharp-acceptplayersession"></a>

Notifies GameLift that a player with the specified player session ID has connected to the server process and needs validation\. GameLift verifies that the player session ID is valid\. After the player session is validated, GameLift changes the status of the player slot from RESERVED to ACTIVE\. 

### Syntax<a name="integration-server-sdk5-csharp-acceptplayersession-syntax"></a>

```
GenericOutcome AcceptPlayerSession(String playerSessionId)
```

### Parameters<a name="integration-server-sdk5-csharp-acceptplayersession-parameter"></a>

playerSessionId  
Unique ID issued by GameLift when a new player session is created\.

### Return value<a name="integration-server-sdk5-csharp-acceptplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\. 

### Example<a name="integration-server-sdk5-csharp-acceptplayersession-example"></a>

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

## RemovePlayerSession\(\)<a name="integration-server-sdk5-csharp-removeplayersession"></a>

Notifies GameLift that a player has disconnected from the server process\. In response, GameLift changes the player slot to available\. 

### Syntax<a name="integration-server-sdk5-csharp-removeplayersession-syntax"></a>

```
GenericOutcome RemovePlayerSession(String playerSessionId)
```

### Parameters<a name="integration-server-sdk5-csharp-removeplayersession-parameter"></a>

**playerSessionId**  
Unique ID issued by GameLift when a new player session is created\.

### Return value<a name="integration-server-sdk5-csharp-removeplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk5-csharp-removeplayersession-example"></a>

```
Aws::GameLift::GenericOutcome disconnectOutcome = Aws::GameLift::Server::RemovePlayerSession(playerSessionId);
```

## DescribePlayerSessions\(\)<a name="integration-server-sdk5-csharp-describeplayersessions"></a>

Retrieves player session data which includes settings, session metadata, and player data\. Use this action to get information for a single player session, for all player sessions in a game session, or for all player sessions associated with a single player ID\.

### Syntax<a name="integration-server-sdk5-csharp-describeplayersessions-syntax"></a>

```
DescribePlayerSessionsOutcome DescribePlayerSessions(DescribePlayerSessionsRequest describePlayerSessionsRequest)
```

### Parameters<a name="integration-server-sdk5-csharp-describeplayersessions-parameter"></a>

**[DescribePlayerSessionsRequest](integration-server-sdk5-csharp-datatypes.md#integration-server-sdk5-csharp-dataypes-playersessions)**  
A `DescribePlayerSessionsRequest` object describes which player sessions to retrieve\.

### Return value<a name="integration-server-sdk5-csharp-describeplayersessions-return"></a>

If successful, returns a `DescribePlayerSessionsOutcome` object that contains a set of player session objects that fit the request parameters\.

### Example<a name="integration-server-sdk5-csharp-describeplayersessions-example"></a>

This example illustrates a request for all player sessions actively connected to a specified game session\. By omitting *NextToken* and setting the *Limit* value to 10, GameLift will return the first 10 player session records matching the request\.

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
```

## StartMatchBackfill\(\)<a name="integration-server-sdk5-csharp-startmatchbackfill"></a>

Sends a request to find new players for open slots in a game session created with FlexMatch\. For more information, see [ FlexMatch backfill feature](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-backfill.html)\.

This action is asynchronous\. If new players are matched, GameLift delivers updated matchmaker data using the callback function `OnUpdateGameSession()`\.

A server process can have only one active match backfill request at a time\. To send a new request, first call [StopMatchBackfill\(\)](#integration-server-sdk5-csharp-stopmatchbackfill) to cancel the original request\.

### Syntax<a name="integration-server-sdk5-csharp-startmatchbackfill-syntax"></a>

```
StartMatchBackfillOutcome StartMatchBackfill (StartMatchBackfillRequest startBackfillRequest);
```

### Parameters<a name="integration-server-sdk5-csharp-startmatchbackfill-parameter"></a>

**[StartMatchBackfillRequest](integration-server-sdk5-csharp-datatypes.md#integration-server-sdk5-csharp-dataypes-startmatchbackfillrequest)**  
A `StartMatchBackfillRequest` object holds information about the backfill request\.

### Return value<a name="integration-server-sdk5-csharp-startmatchbackfill-return"></a>

Returns a StartMatchBackfillOutcome object with the match backfill ticket ID, or failure with an error message\. 

### Example<a name="integration-server-sdk5-csharp-startmatchbackfill-example"></a>

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

## StopMatchBackfill\(\)<a name="integration-server-sdk5-csharp-stopmatchbackfill"></a>

Cancels an active match backfill request\. For more information, see [FlexMatch backfill feature](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-backfill.html)\.

### Syntax<a name="integration-server-sdk5-csharp-stopmatchbackfill-syntax"></a>

```
GenericOutcome StopMatchBackfill (StopMatchBackfillRequest stopBackfillRequest);
```

### Parameters<a name="integration-server-sdk5-csharp-stopmatchbackfill-parameter"></a>

**[StopMatchBackfillRequest](integration-server-sdk5-csharp-datatypes.md#integration-server-sdk5-csharp-dataypes-stopmatchbackfillrequest)**  
A `StopMatchBackfillRequest` object that provides details about the matchmaking ticket you are stopping\.

### Return value<a name="integration-server-sdk5-csharp-stopmatchbackfill-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk5-csharp-stopmatchbackfill-example"></a>

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

## GetComputeCertificate\(\)<a name="integration-server-sdk5-csharp-getcomputertificate"></a>

Retrieves the path to the TLS certificate used to encrypt the network connection between your GameLift Anywhere compute resource and GameLift\. You can use the certificate path when you register your compute device to a GameLift Anywhere fleet\. For more information see, [RegisterCompute](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html)\.

### Syntax<a name="integration-server-sdk5-csharp-getcomputertificate-syntax"></a>

```
GetComputeCertificateOutcome GetComputeCertificate();
```

### Return value<a name="integration-server-sdk5-csharp-getcomputertificate-return"></a>

Returns a GetComputeCertificateResponse object that contains the following: 
+ CertificatePath: The path to the TLS certificate on your compute resource\. 
+ HostName: The hostname of your compute resource\.

### Example<a name="integration-server-sdk5-csharp-getfleetrolecredentials"></a>

```
var getComputeCertificateOutcome = GameLiftServerAPI.GetComputeCertificate();
```

## GetFleetRoleCredentials\(\)<a name="integration-server-sdk5-csharp-getfleetrolecredentials"></a>

Retrieves the service role credentials you create to extend permissions to your other AWS services to GameLift\. These credentials allow your game server to use your AWS resources\. For more information, see \.[Set up an IAM service role for GameLift](setting-up-role.md)\.

### Syntax<a name="integration-server-sdk5-csharp-getfleetrolecredentials-syntax"></a>

```
GetFleetRoleCredentialsOutcome GetFleetRoleCredentials(GetFleetRoleCredentialsRequest request);
```

### Parameters<a name="integration-server-sdk5-csharp-getfleetrolecredentials-parameters"></a>

[GetFleetRoleCredentialsRequest](integration-server-sdk5-csharp-datatypes.md#integration-server-sdk5-csharp-dataypes-getfleetrolecredentialsrequest)  
Role credentials that extend limited access to your AWS resources to the game server\.

### Return value<a name="integration-server-sdk5-csharp-getfleetrolecredentials-return"></a>

Returns a GetFleetRoleCredentialsOutcome object that contains the following: 
+ AssumedRoleUserArn \- The Amazon Resource Name \(ARN\) of the user that the service role belongs to\. 
+ AssumedRoleId \- The ID of the user that the service role belongs to\. 
+ AccessKeyId \- The access key ID to authenticate and provide access to your AWS resources\. 
+ SecretAccessKey \- The secret access key ID for authentication\. 
+ SessionToken \- A token to identify the current active session interacting with your AWS resources\. 
+ Expiration \- The amount of time until your session credentials expire\.

### Example<a name="integration-server-sdk5-csharp-getfleetrolecredentials-example"></a>

```
// form the fleet credentials request  
var getFleetRoleCredentialsRequest = new AWS.GameLift.Server.Model.GetFleetRoleCredentialsRequest(){  
    RoleArn = "arn:aws:iam::123456789012:role/service-role/exampleGameLiftAction"  
 }  

var GetFleetRoleCredentialsOutcome credentials = GetFleetRoleCredentials(getFleetRoleCredentialsRequest)
```

## Destroy\(\)<a name="integration-server-sdk5-csharp-destroy"></a>

Deletes the instance of the GameLift game server SDK on your resource\. This removes all state information, stops heartbeat communication with GameLift, stops game session management, and closes any connections\. Call this after you've ended the server processes\. 

### Syntax<a name="integration-server-sdk5-csharp-destroy-syntax"></a>

```
GenericOutcome Destroy()
```

### Return value<a name="integration-server-sdk5-csharp-destroy-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk5-csharp-destroy-example"></a>

```
// Operations to end game sessions and the server process
var processEndingOutcome = GameLiftServerAPI.ProcessEnding();
if (processReadyOutcome.Success)
    Environment.Exit(0);
// Otherwise, exit with error code
Environment.Exit(errorCode);

// Shutdown and destroy the intance of the GameLift Game Server SDK
var destroyOutcome = GameLiftServerAPI.Destroy();
```