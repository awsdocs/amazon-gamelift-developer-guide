# GameLift and game client/server interactions<a name="gamelift-sdk-interactions"></a>

This topic describes the interactions between a client app service, a game server, and the GameLift service\. See also the [Amazon GameLift–Game Server/Client Interactions](gamelift-sdk-server-api-interaction-vsd.md) diagram\. 

## Setting up a new server process<a name="gamelift-sdk-interactions-launch"></a>

1. The **GameLift service** launches a new server process on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\.

1. The **server process**, as part of the launch process, calls these Server API actions:
   + `InitSDK()` to initialize the server SDK\.
   + `ProcessReady()` to communicate readiness to accept a game session and specify connection port and location of game session log files\.

   The server process then waits for a callback from the GameLift service\.

1. The **GameLift service** updates the status of the server process to ACTIVE to allow placement of game sessions with the server process\. \(If the server process is the first one to become active on an instance, the instance status is also updated to ACTIVE\.\)

1. The **GameLift service** begins calling the `onHealthCheck` callback and continues to call it periodically while the server process is active\. The server process can report either healthy or not healthy within one minute\.

## Creating a game session<a name="gamelift-sdk-interactions-start"></a>

1. The **Client app** calls the client API action `StartGameSessionPlacement()`\.

1. The **GameLift service** creates a new `GameSessionPlacement` ticket with status PENDING and returns it to the requesting client app\.

1. The **Client app** obtains placement ticket status from a queue\. For more information, see [Set up event notification for game session placement](queue-notification.md)\. 

1. The **GameLift service** intitates game session placement, selecting an appropriate fleet and searching for an active server process in the fleet with 0 game sessions\. When a server process is located, GameLift does the following: 
   + Creates a `GameSession` object with the game session settings and player data from the placement request and status ACTIVATING\.
   + Invokes the `onStartGameSession` callback on the server process\. It passes the `GameSession` object with information that the server process may need to set up the game session\.
   + Changes the server process's number of game sessions to 1\.

1. The **server process** runs the `onStartGameSession` callback function\. When ready to accept player connections, the server process calls `ActivateGameSession()` and waits for player connections\.

1. The **GameLift service** does the following: 
   + Updates the `GameSession` object with connection information for the server process \(including the port setting that was reported with `ProcessReady()`\) and changes the status to ACTIVE\.
   + Updates the `GameSessionPlacement` ticket with the connection information and sets the ticket status to FULFILLED\.

1. The **Client app** detects the updated ticket status and is able to use the connection information to connect to the server process and join the game session\.

## Adding a player to a game session<a name="gamelift-sdk-interactions-add-player"></a>

This sequence describes the process of adding a player to an existing game session\. Player sessions can also be requested as part of a game session placement request\.

1. The **Client app** calls the client API action `CreatePlayerSession()` with a game session ID\.

1. The **GameLift service** checks the game session status \(must be ACTIVE\), and looks for an open player slot in the game session\. If a slot is available, it does the following:
   + Creates a new `PlayerSession` object and sets its status to RESERVED\.
   + Responds to the client app request with the `PlayerSession` object\.

1. The **Client app** connects directly to the server process with the player session ID\.

1. The **server process** calls the Server API action `AcceptPlayerSession()` to validate the player session ID\. If validated, the GameLift service passes the `PlayerSession` object to the server process\. The server process either accepts or rejects the connection\.

1. The **GameLift service** does one of the following:
   + If the connection is accepted, sets the `PlayerSession` status to ACTIVE\.
   + If no response is received within 60 seconds of the client app's original `CreatePlayerSession()` call, changes the `PlayerSession` status to TIMEDOUT and reopens the player slot in the game session\.

## Removing a player from a game session<a name="gamelift-sdk-interactions-remove-player"></a>

1. The **Client app** disconnects from the server process\.

1. The **server process** detects the lost connection and calls the server API action `RemovePlayerSession()`\.

1. The **GameLift service** changes the `PlayerSession` status to COMPLETED and reopens the player slot in the game session\.

## Shutting down a game session<a name="gamelift-sdk-interactions-shutdown"></a>

This sequence is used when a server process is ending the current game session terminating itself\. 

1. The **server process** does the following: 
   + Runs code to gracefully shuts down the game session and the server process\.
   + Calls the server API action `ProcessEnding()` to inform the GameLift service\.

1. The **GameLift service** does the following:
   + Uploads game session logs to Amazon Simple Storage Service \(Amazon S3\)\.
   + Changes the `GameSession` status to TERMINATED\.
   + Changes the server process status to TERMINATED\.
   + Recycles instance resources based on the fleet's runtime configuration\.

## Responding to a shutdown request<a name="gamelift-sdk-interactions-shutdown-request"></a>

This sequence is used by the GameLift service to force a server process to shut down\. This action may be done to end an unhealthy process or to gracefully shut down a process when the instance the process is on is being terminated, such as during autoscaling\. It might also be used when handling a Spot Instance interruption\.

1. The **GameLift service** invokes the server process's `onProcessTerminate` callback\. 

1. The **server process** runs the `onProcessTerminate` callback function, which triggers the process's termination sequence, ending with a call to `ProcessEnding()`\. 

1. The **GameLift service** does the following, either in response to receiving the `ProcessEnding()` call or after five minutes: 
   + If a game session was in progress, uploads game session logs \(if any\) to Amazon S3 and changes the `GameSession` status to TERMINATED\.
   + Changes the server process status to TERMINATED\.
   + Recycles instance resources based on the fleet's runtime configuration\.