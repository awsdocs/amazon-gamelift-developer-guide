# Amazon GameLift Server API \(C\+\+\) Reference: Actions<a name="integration-server-sdk-cpp-ref-actions"></a>

This Amazon GameLift C\+\+ Server API reference can help you prepare your multiplayer game for use with Amazon GameLift\. For details on the integration process, see [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)\.

This API is defined in `GameLiftServerAPI.h`, `LogParameters.h`, `ProcessParameters.h` \.
+ Actions
+ [Data Types](integration-server-sdk-cpp-ref-datatypes.md)

## AcceptPlayerSession\(\)<a name="integration-server-sdk-cpp-ref-acceptplayersession"></a>

Notifies the Amazon GameLift service that a player with the specified player session ID has connected to the server process and needs validation\. Amazon GameLift verifies that the player session ID is valid—that is, that the player ID has reserved a player slot in the game session\. Once validated, Amazon GameLift changes the status of the player slot from RESERVED to ACTIVE\. 

### Syntax<a name="integration-server-sdk-cpp-ref-acceptplayersession-syntax"></a>

```
GenericOutcome AcceptPlayerSession(const std::string& playerSessionId);
```

### Parameters<a name="integration-server-sdk-cpp-ref-acceptplayersession-parameter"></a>

**playerSessionId**  
Unique ID issued by the Amazon GameLift service in response to a call to the AWS SDK Amazon GameLift API action [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html)\. The game client references this ID when connecting to the server process\.  
Type: std::string  
Required: No

### Return Value<a name="integration-server-sdk-cpp-ref-acceptplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\. 

### Example<a name="integration-server-sdk-cpp-ref-acceptplayersession-example"></a>

This example illustrates a function for handling a connection request, including validating and rejecting invalid player session IDs\. 

```
void ReceiveConnectingPlayerSessionID (Connection& connection, const std::string& playerSessionId){
    Aws::GameLift::GenericOutcome connectOutcome = 
        Aws::GameLift::Server::AcceptPlayerSession(playerSessionId);
    if(connectOutcome.IsSuccess())
    {
        connectionToSessionMap.emplace(connection, playerSessionId);
        connection.Accept();
    }
    else 
    {
        connection.Reject(connectOutcome.GetError().GetMessage();
    }       
}
```

## ActivateGameSession\(\)<a name="integration-server-sdk-cpp-ref-activategamesession"></a>

Notifies the Amazon GameLift service that the server process has started a game session and is now ready to receive player connections\. This action should be called as part of the `onStartGameSession()` callback function, after all game session initialization has been completed\.

### Syntax<a name="integration-server-sdk-cpp-ref-activategamesession-syntax"></a>

```
GenericOutcome ActivateGameSession();
```

### Parameters<a name="integration-server-sdk-cpp-ref-activategamesession-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-activategamesession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-activategamesession-example"></a>

This example shows `ActivateGameSession()` being called as part of the `onStartGameSession()` callback function\. 

```
void onStartGameSession(Aws::GameLift::Model::GameSession myGameSession)
{
   // game-specific tasks when starting a new game session, such as loading map
   GenericOutcome outcome = Aws::GameLift::Server::ActivateGameSession();
}
```

## DescribePlayerSessions\(\)<a name="integration-server-sdk-cpp-ref-describeplayersessions"></a>

Retrieves player session data, including settings, session metadata, and player data\. Use this action to get information for a single player session, for all player sessions in a game session, or for all player sessions associated with a single player ID\.

### Syntax<a name="integration-server-sdk-cpp-ref-describeplayersessions-syntax"></a>

```
DescribePlayerSessionsOutcome DescribePlayerSessions ( 
    const Aws::GameLift::Server::Model::DescribePlayerSessionsRequest &describePlayerSessionsRequest);
```

### Parameters<a name="integration-server-sdk-cpp-ref-describeplayersessions-parameter"></a>

**describePlayerSessionsRequest**  
A [DescribePlayerSessionsRequest](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-playersessions) object describing which player sessions to retrieve\.  
Required: Yes

### Return Value<a name="integration-server-sdk-cpp-ref-describeplayersessions-return"></a>

If successful, returns a `DescribePlayerSessionsOutcome` object containing a set of player session objects that fit the request parameters\. Player session objects have a structure identical to the AWS SDK Amazon GameLift API [PlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_PlayerSession.html) data type\.

### Example<a name="integration-server-sdk-cpp-ref-describeplayersessions-example"></a>

This example illustrates a request for all player sessions actively connected to a specified game session\. By omitting `NextToken` and setting the `Limit` value to 10, Amazon GameLift returns the first 10 player sessions records matching the request\.

```
// Set request parameters
Aws::GameLift::Server::Model::DescribePlayerSessionsRequest request;
request.SetPlayerSessionStatusFilter(Aws::GameLift::Server::Model::PlayerSessionStatusMapper::GetNameForPlayerSessionStatus(Aws::GameLift::Server::Model::PlayerSessionStatus::Active));
request.SetLimit(10);
request.SetGameSessionId("the game session ID");    // can use GetGameSessionId()

// Call DescribePlayerSessions
Aws::GameLift::DescribePlayerSessionsOutcome playerSessionsOutcome = 
    Aws::GameLift::Server::DescribePlayerSessions(request);
```

## GetGameSessionId\(\)<a name="integration-server-sdk-cpp-ref-getgamesessionid"></a>

Retrieves a unique identifier for the game session currently being hosted by the server process, if the server process is active\. The identifier is returned in ARN format: `arn:aws:gamelift:<region>::gamesession/fleet-<fleet ID>/<ID string>`\. 

### Syntax<a name="integration-server-sdk-cpp-ref-getgamesessionid-syntax"></a>

```
AwsStringOutcome GetGameSessionId();
```

### Parameters<a name="integration-server-sdk-cpp-ref-getgamesessionid-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-getgamesessionid-return"></a>

If successful, returns the game session ID as an `AwsStringOutcome` object\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-cpp-ref-getgamesessionid-example"></a>

```
Aws::GameLift::AwsStringOutcome sessionIdOutcome = 
    Aws::GameLift::Server::GetGameSessionId();
```

## GetInstanceCertificate\(\)<a name="integration-server-sdk-cpp-ref-getinstancecertificate"></a>

Retrieves the file location of a pem\-encoded TLS certificate that is associated with the fleet and its instances\. This certificate is generated when a new fleet is created with the certificate configuration set to GENERATED\. Use this certificate to establish a secure connection with a game client and to encrypt client/server communication\. 

### Syntax<a name="integration-server-sdk-cpp-ref-getinstancecertificate-syntax"></a>

```
GetInstanceCertificateOutcome GetInstanceCertificate();
```

### Parameters<a name="integration-server-sdk-cpp-ref-getinstancecertificate-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-getinstancecertificate-return"></a>

If successful, returns a `GetInstanceCertificateOutcome` object containing the location of the fleet's TLS certificate file, which is stored on the instance\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-cpp-ref-getinstancecertificate-example"></a>

```
Aws::GameLift::GetInstanceCertificateOutcome certificateOutcome = 
    Aws::GameLift::Server::GetInstanceCertificate();
```

## GetSdkVersion\(\)<a name="integration-server-sdk-cpp-ref-getsdk"></a>

Returns the current version number of the SDK in use\.

### Syntax<a name="integration-server-sdk-cpp-ref-getsdk-syntax"></a>

```
AwsStringOutcome GetSdkVersion();
```

### Parameters<a name="integration-server-sdk-cpp-ref-getsdk-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-getsdk-return"></a>

If successful, returns the current SDK version as an `AwsStringOutcome` object\. The returned string includes the version number only \(ex\. "3\.1\.5"\)\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-cpp-ref-getsdk-example"></a>

```
Aws::GameLift::AwsStringOutcome SdkVersionOutcome = 
    Aws::GameLift::Server::GetSdkVersion();
```

## GetTerminationTime\(\)<a name="integration-server-sdk-cpp-ref-getterm"></a>

Returns the time that a server process is scheduled to be shut down, if a termination time is available\. A server process takes this action after receiving an `onProcessTerminate()` callback from the Amazon GameLift service\. A server process may be shut down for several reasons: \(1\) process poor health, \(2\) when an instance is being terminated during a scale\-down event, or \(3\) when an instance is being terminated due to a [spot\-instance interruption](spot-tasks.md)\. 

If the process has received an `onProcessTerminate()` callback, the value returned is the estimated termination time in epoch seconds\. If no termination time is available, the value returned is \-1, which indicates that the server process may be terminated at any time\. If the process has not received an `onProcessTerminate()` callback, the returned value will always be \-1\. Learn more about [shutting down a server process](gamelift-sdk-server-api.md#gamelift-sdk-server-terminate)\.

### Syntax<a name="integration-server-sdk-cpp-ref-getterm-syntax"></a>

```
AwsLongOutcome GetTerminationTime();
```

### Parameters<a name="integration-server-sdk-cpp-ref-getterm-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-getterm-return"></a>

If successful, returns the current SDK version as an `AwsLongOutcome` object\. The value is either the termination time in epoch seconds, or the value \-1\. If not successful, returns an error message\.

### Example<a name="integration-server-sdk-cpp-ref-getterm-example"></a>

```
Aws::GameLift::AwsLongOutcome TermTimeOutcome = 
    Aws::GameLift::Server::GetTerminationTime();
```

## InitSDK\(\)<a name="integration-server-sdk-cpp-ref-initsdk"></a>

Initializes the Amazon GameLift SDK\. This method should be called on launch, before any other Amazon GameLift\-related initialization occurs\.

### Syntax<a name="integration-server-sdk-cpp-ref-initsdk-syntax"></a>

```
InitSDKOutcome InitSDK();
```

### Parameters<a name="integration-server-sdk-cpp-ref-initsdk-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-initsdk-return"></a>

If successful, returns an InitSdkOutcome object indicating that the server process is ready to call [ProcessReady\(\)](#integration-server-sdk-cpp-ref-processready)\. 

### Example<a name="integration-server-sdk-cpp-ref-initsdk-example"></a>

```
Aws::GameLift::Server::InitSDKOutcome initOutcome = 
    Aws::GameLift::Server::InitSDK();
```

## ProcessEnding\(\)<a name="integration-server-sdk-cpp-ref-processending"></a>

Notifies the Amazon GameLift service that the server process is shutting down\. This method should exit with an exit code of 0; a non\-zero exit code results in an event message that the process did not exit cleanly\.

### Syntax<a name="integration-server-sdk-cpp-ref-processending-syntax"></a>

```
GenericOutcome ProcessEnding();
```

### Parameters<a name="integration-server-sdk-cpp-ref-processending-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-processending-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-processending-example"></a>

```
Aws::GameLift::GenericOutcome outcome = Aws::GameLift::Server::ProcessEnding();
```

## ProcessReady\(\)<a name="integration-server-sdk-cpp-ref-processready"></a>

Notifies the Amazon GameLift service that the server process is ready to host game sessions\. This method should be called after successfully invoking [InitSDK\(\)](#integration-server-sdk-cpp-ref-initsdk) and completing any setup tasks required before the server process can host a game session\.

This call is synchronous\. To make an asynchronous call, use [ProcessReadyAsync\(\)](#integration-server-sdk-cpp-ref-processreadyasync)\. See [Prepare a Server Process](gamelift-sdk-server-api.md#gamelift-sdk-server-initialize) for more details\.

### Syntax<a name="integration-server-sdk-cpp-ref-processready-syntax"></a>

```
GenericOutcome ProcessReady(
    const Aws::GameLift::Server::ProcessParameters &processParameters);
```

### Parameters<a name="integration-server-sdk-cpp-ref-processready-parameter"></a>

**processParameters**  
A [ProcessParameters](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-process) object communicating the following information about the server process:  
+ Names of callback methods, implemented in the game server code, that the Amazon GameLift service invokes to communicate with the server process\.
+ Port number that the server process is listening on\.
+ Path to any game session\-specific files that you want Amazon GameLift to capture and store\.
Required: Yes

### Return Value<a name="integration-server-sdk-cpp-ref-processready-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-processready-example"></a>

This example illustrates both the [ProcessReady\(\)](#integration-server-sdk-cpp-ref-processready) call and callback function implementations\.

```
// Set parameters and call ProcessReady
std::string serverLog("serverOut.log");        // Example of a log file written by the game server
std::vector<std::string> logPaths;
logPaths.push_back(serverLog);

int listenPort = 9339;

Aws::GameLift::Server::ProcessParameters processReadyParameter = Aws::GameLift::Server::ProcessParameters(
    std::bind(&Server::onStartGameSession, this, std::placeholders::_1),
    std::bind(&Server::onProcessTerminate, this),
    std::bind(&Server::OnHealthCheck, this),
    std::bind(&Server::OnUpdateGameSession, this),
    listenPort,
    Aws::GameLift::Server::LogParameters(logPaths)); 

Aws::GameLift::GenericOutcome outcome = 
   Aws::GameLift::Server::ProcessReady(processReadyParameter);

// Implement callback functions
void Server::onStartGameSession(Aws::GameLift::Model::GameSession myGameSession)
{
   // game-specific tasks when starting a new game session, such as loading map
   GenericOutcome outcome = 
       Aws::GameLift::Server::ActivateGameSession (maxPlayers);
}

void Server::onProcessTerminate()
{
   // game-specific tasks required to gracefully shut down a game session, 
   // such as notifying players, preserving game state data, and other cleanup
   GenericOutcome outcome = Aws::GameLift::Server::ProcessEnding();
}

bool Server::onHealthCheck()
{
    bool health;
    // complete health evaluation within 60 seconds and set health
    return health;
}
```

## ProcessReadyAsync\(\)<a name="integration-server-sdk-cpp-ref-processreadyasync"></a>

Notifies the Amazon GameLift service that the server process is ready to host game sessions\. This method should be called once the server process is ready to host a game session\. The parameters specify the names of callback functions for Amazon GameLift to call in certain circumstances\. Game server code must implement these functions\.

This call is asynchronous\. To make a synchronous call, use [ProcessReady\(\)](#integration-server-sdk-cpp-ref-processready)\. See [Prepare a Server Process](gamelift-sdk-server-api.md#gamelift-sdk-server-initialize) for more details\.

### Syntax<a name="integration-server-sdk-cpp-ref-processreadyasync-syntax"></a>

```
GenericOutcomeCallable ProcessReadyAsync(
    const Aws::GameLift::Server::ProcessParameters &processParameters);
```

### Parameters<a name="integration-server-sdk-cpp-ref-processreadyasync-parameter"></a>

**processParameters**  
A [ProcessParameters](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-process) object communicating the following information about the server process:  
+ Names of callback methods, implemented in the game server code, that the Amazon GameLift service invokes to communicate with the server process\.
+ Port number that the server process is listening on\.
+ Path to any game session\-specific files that you want Amazon GameLift to capture and store\.
Required: Yes

### Return Value<a name="integration-server-sdk-cpp-ref-processreadyasync-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-processreadyasync-example"></a>

```
// Set parameters and call ProcessReady
std::string serverLog("serverOut.log");        // This is an example of a log file written by the game server
std::vector<std::string> logPaths;
logPaths.push_back(serverLog);

int listenPort = 9339;

Aws::GameLift::Server::ProcessParameters processReadyParameter = Aws::GameLift::Server::ProcessParameters(
    std::bind(&Server::onStartGameSession, this, std::placeholders::_1),
    std::bind(&Server::onProcessTerminate, this),
    std::bind(&Server::OnHealthCheck, this),
    std::bind(&Server::OnUpdateGameSession, this),
    listenPort,
    Aws::GameLift::Server::LogParameters(logPaths));

Aws::GameLift::GenericOutcomeCallable outcome = 
   Aws::GameLift::Server::ProcessReadyAsync(processReadyParameter);

// Implement callback functions
void onStartGameSession(Aws::GameLift::Model::GameSession myGameSession)
{
   // game-specific tasks when starting a new game session, such as loading map
   GenericOutcome outcome = Aws::GameLift::Server::ActivateGameSession (maxPlayers);
}

void onProcessTerminate()
{
   // game-specific tasks required to gracefully shut down a game session, 
   // such as notifying players, preserving game state data, and other cleanup
   GenericOutcome outcome = Aws::GameLift::Server::ProcessEnding();
}

bool onHealthCheck()
{
    // perform health evaluation and complete within 60 seconds
    return health;
}
```

## RemovePlayerSession\(\)<a name="integration-server-sdk-cpp-ref-removeplayersession"></a>

Notifies the Amazon GameLift service that a player with the specified player session ID has disconnected from the server process\. In response, Amazon GameLift changes the player slot to available, which allows it to be assigned to a new player\. 

### Syntax<a name="integration-server-sdk-cpp-ref-removeplayersession-syntax"></a>

```
GenericOutcome RemovePlayerSession(
    const std::string& playerSessionId);
```

### Parameters<a name="integration-server-sdk-cpp-ref-removeplayersession-parameter"></a>

**playerSessionId**  
Unique ID issued by the Amazon GameLift service in response to a call to the AWS SDK Amazon GameLift API action [CreatePlayerSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreatePlayerSession.html)\. The game client references this ID when connecting to the server process\.  
Type: std::string  
Required: No

### Return Value<a name="integration-server-sdk-cpp-ref-removeplayersession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-removeplayersession-example"></a>

```
Aws::GameLift::GenericOutcome disconnectOutcome = 
    Aws::GameLift::Server::RemovePlayerSession(playerSessionId);
```

## StartMatchBackfill\(\)<a name="integration-server-sdk-cpp-ref-startmatchbackfill"></a>

Sends a request to find new players for open slots in a game session created with FlexMatch\. See also the AWS SDK action [StartMatchBackfill\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchBackfill.html)\. With this action, match backfill requests can be initiated by a game server process that is hosting the game session\. Learn more about the FlexMatch backfill feature in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

This action is asynchronous\. If new players are successfully matched, the Amazon GameLift service delivers updated matchmaker data using the callback function `OnUpdateGameSession()`\.

A server process can have only one active match backfill request at a time\. To send a new request, first call [StopMatchBackfill\(\)](#integration-server-sdk-cpp-ref-stopmatchbackfill) to cancel the original request\.

### Syntax<a name="integration-server-sdk-cpp-ref-startmatchbackfill-syntax"></a>

```
StartMatchBackfillOutcome StartMatchBackfill ( 
    const Aws::GameLift::Server::Model::StartMatchBackfillRequest &startBackfillRequest);
```

### Parameters<a name="integration-server-sdk-cpp-ref-startmatchbackfill-parameter"></a>

**StartMatchBackfillRequest**  
A [StartMatchBackfillRequest](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-startmatchbackfillrequest) object that communicates the following information:  
+ A ticket ID to assign to the backfill request\. This information is optional; if no ID is provided, Amazon GameLift will autogenerate one\.
+ The matchmaker to send the request to\. The full configuration ARN is required\. This value can be acquired from the game session's matchmaker data\.
+ The ID of the game session that is being backfilled\.
+ Available matchmaking data for the game session's current players\.
Required: Yes

### Return Value<a name="integration-server-sdk-cpp-ref-startmatchbackfill-return"></a>

Returns a StartMatchBackfillOutcome object with the match backfill ticket or failure with an error message\. Ticket status can be tracked using the AWS SDK action [DescribeMatchmaking\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html)\.

### Example<a name="integration-server-sdk-cpp-ref-startmatchbackfill-example"></a>

```
// Build a backfill request
std::vector<Player> players;
Aws::GameLift::Server::Model::StartMatchBackfillRequest startBackfillRequest;
startBackfillRequest.SetTicketId("a ticket ID");                                         //optional, autogenerated if not provided
startBackfillRequest.SetMatchmakingConfigurationArn("the matchmaker configuration ARN"); //from the game session matchmaker data
startBackfillRequest.SetGameSessionArn("the game session ARN");                          // can use GetGameSessionId()
startBackfillRequest.SetPlayers(players);                                                  //from the game session matchmaker data

// Send backfill request
Aws::GameLift::StartMatchBackfillOutcome backfillOutcome = 
    Aws::GameLift::Server::StartMatchBackfill(startBackfillRequest);

// Implement callback function for backfill
void Server::OnUpdateGameSession(Aws::GameLift::Server::Model::GameSession gameSession, Aws::GameLift::Server::Model::UpdateReason updateReason, std::string backfillTicketId)
{
   // handle status messages
   // perform game-specific tasks to prep for newly matched players
}
```

## StopMatchBackfill\(\)<a name="integration-server-sdk-cpp-ref-stopmatchbackfill"></a>

Cancels an active match backfill request that was created with [StartMatchBackfill\(\)](#integration-server-sdk-cpp-ref-startmatchbackfill)\. See also the AWS SDK action [StopMatchmaking\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html)\. Learn more about the FlexMatch backfill feature in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

### Syntax<a name="integration-server-sdk-cpp-ref-stopmatchbackfill-syntax"></a>

```
GenericOutcome StopMatchBackfill ( 
    const Aws::GameLift::Server::Model::StopMatchBackfillRequest &stopBackfillRequest);
```

### Parameters<a name="integration-server-sdk-cpp-ref-stopmatchbackfill-parameter"></a>

**StopMatchBackfillRequest**  
A [StopMatchBackfillRequest](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-stopmatchbackfillrequest) object identifying the matchmaking ticket to cancel:   
+ ticket ID assigned to the backfill request being canceled
+ matchmaker the backfill request was sent to
+ game session associated with the backfill request
Required: Yes

### Return Value<a name="integration-server-sdk-cpp-ref-stopmatchbackfill-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-stopmatchbackfill-example"></a>

```
// Set backfill stop request parameters

Aws::GameLift::Server::Model::StopMatchBackfillRequest stopBackfillRequest;
stopBackfillRequest.SetTicketId("the ticket ID");
stopBackfillRequest.SetGameSessionArn("the game session ARN");                           // can use GetGameSessionId()
stopBackfillRequest.SetMatchmakingConfigurationArn("the matchmaker configuration ARN");  // from the game session matchmaker data

Aws::GameLift::GenericOutcome stopBackfillOutcome = 
    Aws::GameLift::Server::StopMatchBackfillRequest(stopBackfillRequest);
```

## TerminateGameSession\(\)<a name="integration-server-sdk-cpp-ref-terminategamesession"></a>

Notifies the Amazon GameLift service that the server process has shut down the game session\. Since each server process hosts only one game session at a time, there's no need to specify which session\. This action should be called at the end of the game session shutdown process\. After calling this action, the server process can call [ProcessReady\(\)](#integration-server-sdk-cpp-ref-processready) to signal its availability to host a new game session\. Alternatively it can call [ProcessEnding\(\)](#integration-server-sdk-cpp-ref-processending) to shut down the server process and terminate the instance\.

### Syntax<a name="integration-server-sdk-cpp-ref-terminategamesession-syntax"></a>

```
GenericOutcome TerminateGameSession();
```

### Parameters<a name="integration-server-sdk-cpp-ref-terminategamesession-parameter"></a>

This action has no parameters\.

### Return Value<a name="integration-server-sdk-cpp-ref-terminategamesession-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-terminategamesession-example"></a>

This example illustrates a server process at the end of a game session\.

```
// game-specific tasks required to gracefully shut down a game session, 
// such as notifying players, preserving game state data, and other cleanup

Aws::GameLift::GenericOutcome outcome = 
    Aws::GameLift::Server::TerminateGameSession();
Aws::GameLift::GenericOutcome outcome = 
    Aws::GameLift::Server::ProcessReady(onStartGameSession, onProcessTerminate);
```

## UpdatePlayerSessionCreationPolicy\(\)<a name="integration-server-sdk-cpp-ref-updateplayersessioncreationpolicy"></a>

Updates the current game session's ability to accept new player sessions\. A game session can be set to either accept or deny all new player sessions\. See also the AWS SDK action [UpdateGameSession\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html)\.

### Syntax<a name="integration-server-sdk-cpp-ref-updateplayersessioncreationpolicy-syntax"></a>

```
GenericOutcome UpdatePlayerSessionCreationPolicy(
    Aws::GameLift::Model::PlayerSessionCreationPolicy newPlayerSessionPolicy);
```

### Parameters<a name="integration-server-sdk-cpp-ref-updateplayersessioncreationpolicy-parameter"></a>

**newPlayerSessionPolicy**  
String value indicating whether the game session accepts new players\.   
Type: [ Aws::GameLift::Model::PlayerSessionCreationPolicy](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift_1_1_model.html#afa8a7527defe9e7ca0caebc239182c53) enum\. Valid values include:   
+ **ACCEPT\_ALL** – Accept all new player sessions\.
+ **DENY\_ALL** – Deny all new player sessions\.
Required: Yes

### Return Value<a name="integration-server-sdk-cpp-ref-updateplayersessioncreationpolicy-return"></a>

Returns a generic outcome consisting of success or failure with an error message\.

### Example<a name="integration-server-sdk-cpp-ref-updateplayersessioncreationpolicy-example"></a>

This example sets the current game session's join policy to accept all players\.

```
Aws::GameLift::GenericOutcome outcome = 
    Aws::GameLift::Server::UpdatePlayerSessionCreationPolicy(Aws::GameLift::Model::PlayerSessionCreationPolicy::ACCEPT_ALL);
```