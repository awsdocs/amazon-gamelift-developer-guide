# Amazon GameLift and Game Client/Server Interactions<a name="gamelift-sdk-interactions"></a>

This topic describes the interactions between a client app, a game server, and the Amazon GameLift service\. See also the [Amazon GameLift–Game Server/Client Interactions](gamelift-sdk-server-api-interaction-vsd.md) diagram\. 

## Setting Up a New Server Process<a name="gamelift-sdk-interactions-launch"></a>

1. The **Amazon GameLift service** launches a new server process on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\.

1. The **server process**, as part of the launch process, calls these server API actions:
   + `InitSDK()` to initialize the server SDK\.
   + `ProcessReady()` to communicate readiness to accept a game session and specify connection port and location of game session log files\.

   It then waits for a callback from the Amazon GameLift service\.

1. The **Amazon GameLift service** changes the status of the EC2 instance to ACTIVE, with 0 game sessions and 0 players\.

1. The **Amazon GameLift service** begins calling the `onHealthCheck` callback regularly while the server process is active\. The server process must report either healthy or not healthy within one minute\.

## Creating a Game Session<a name="gamelift-sdk-interactions-start"></a>

1. The **Client app** calls the client API action `CreateGameSession()`\.

1. The **Amazon GameLift service** searches for an active server with 0 game sessions\. When found, it does the following: 
   + Creates a new `GameSession` object, using the port setting reported by the server process in `ProcessReady()`, and sets its status to ACTIVATING\.
   + Responds to the client app request with the `GameSession` object\.
   + Invokes the `onStartGameSession` callback on the server process, passing the `GameSession` object\.

1. The **server process** runs the `onStartGameSession` callback function\. When ready to accept player connections, the server process calls the server API action `ActivateGameSession()` and waits for player connections\.

1. The **Amazon GameLift service** changes the `GameSession` status to ACTIVE\.

## Adding a Player to a Game Session<a name="gamelift-sdk-interactions-add-player"></a>

1. The **Client app** calls the client API action `CreatePlayerSession()` with a game session ID\.

1. The **Amazon GameLift service** checks the game session status \(must be ACTIVE\), and looks for an open player slot in the game session\. If a slot is available, it does the following:
   + Creates a new `PlayerSession` object and sets its status to RESERVED\.
   + Responds to the client app request with the `PlayerSession` object\.

1. The **Client app** connects directly to the server process with the player session ID\.

1. The **server process** calls the Server API action `AcceptPlayerSession()` to validate the player session ID\. If validated, the Amazon GameLift service passes the `PlayerSession` object to the server process\. The server process either accepts or rejects the connection\.

1. The **Amazon GameLift service** does one of the following:
   + If the connection is accepted, sets the `PlayerSession` status to ACTIVE\.
   + If no response is received within 60 seconds of the client app's original `CreatePlayerSession()` call, changes the `PlayerSession` status to TIMEDOUT and reopens the player slot in the game session\.

## Removing a Player From a Game Session<a name="gamelift-sdk-interactions-remove-player"></a>

1. The **Client app** disconnects from the server process\.

1. The **server process** detects the lost connection and calls the server API action `RemovePlayerSession()`\.

1. The **Amazon GameLift service** changes the `PlayerSession` status to COMPLETED and reopens the player slot in the game session\.

## Shutting down a Game Session<a name="gamelift-sdk-interactions-shutdown"></a>

**Shutting down a game session**

1. The **server process** calls the server API action `TerminateGameSession()`\. 

1. The **Amazon GameLift service** does the following:
   + Changes the `GameSession` status to TERMINATED\.
   + Uploads game session logs to Amazon Simple Storage Service \(Amazon S3\)\.
   + Updates fleet utilization to indicate the server is idle\(0 game sessions, 0 players\)\.

## Terminating a Server Process<a name="gamelift-sdk-interactions-terminate"></a>

1. The **server process** does the following: 
   + Runs code that gracefully shuts down the server process\.
   + Calls the server API action `ProcessEnding()` to inform the Amazon GameLift service\.

1. The **Amazon GameLift service** does the following:
   + Uploads game session logs \(if any\) to Amazon S3\.
   + Changes the server process status to TERMINATED\.
   + Recycles instance resources based on the fleet's runtime configuration\.

## Responding to a Shutdown Request<a name="gamelift-sdk-interactions-shutdown-request"></a>

1. The **Amazon GameLift service** invokes the server process's `onProcessTerminate` callback\. This call is used to shut down a server process that has reported unhealthy or not responded with health status for three consecutive minutes\.

1. The **server process** runs the `onProcessTerminate` callback function, which triggers the server's termination process, ending with a call to `ProcessEnding()`\. 

1. The **Amazon GameLift service** does the following, either in response to receiving the `ProcessEnding()` call or after five minutes: 
   + Uploads game session logs \(if any\) to Amazon S3\.
   + Changes the server process status to TERMINATED\.
   + Recycles instance resources based on the fleet's runtime configuration\.