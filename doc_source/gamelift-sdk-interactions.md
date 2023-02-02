# GameLift and game client server interactions<a name="gamelift-sdk-interactions"></a>

This topic describes the interactions between the game client, a backend service, a game server, and Amazon GameLift\.

The following diagram illustrates interactions between the game client, backend service, GameLift SDK, managed EC2 game server, GameLift server SDK, and GameLift\. For a detailed description of the interactions shown, see the following sections on this page\.

![\[Game client/server interactions for the use cases listed in the following sections.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/combined_api_interactions_vsd.png)

## Initialize a game server<a name="gamelift-sdk-interactions-launch"></a>

The following steps describe the interactions that occur when you prepare your game server to host game sessions\.

1. GameLift launches the server executable on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\.

1. The game server calls:

   1. `InitSDK()` to initialize the server SDK\.

   1. `ProcessReady()` to communicate game session readiness, connection information, and location of game session log files\.

   The server process then waits for a callback from GameLift\.

1. GameLift updates the status of the server process to `ACTIVE` to enable game session placement\.

1. GameLift begins calling the `onHealthCheck` callback and continues to call it periodically while the server process is active\. The server process can report healthy or not healthy within one minute\.

## Create a game session<a name="gamelift-sdk-interactions-start"></a>

After you've initialized your game server, the following interactions occur when you create game sessions to host your players\.

1. The backend service calls the SDK operation `StartGameSessionPlacement()`\.

1. GameLift creates a new `GameSessionPlacement` ticket with status `PENDING` and returns it to the backend service\.

1. The backend service obtains a placement ticket status from a queue\. For more information, see [Set up event notification for game session placement](queue-notification.md)\.

1. GameLift starts game session placement by selecting an appropriate fleet and searching for an active server process in a fleet with `0` game sessions\. When GameLift locates a server process, GameLift does the following:

   1. Creates a `GameSession` object with the game session settings and player data from the placement request with an `ACTIVATING` status\.

   1. Invokes the `onStartGameSession` callback on the server process\. GameLift passes information to the `GameSession` object indicating that the server process may set up the game session\.

   1. Changes the server process's number of game sessions to `1`\.

1. The server process runs the `onStartGameSession` callback function\. When the server process is ready to accept player connections, it calls `ActivateGameSession()` and waits for player connections\.

1. GameLift updates the `GameSession` object with connection information for the server process\. \(This information includes the port setting that was reported with `ProcessReady()`\.\) GameLift also changes the status to `ACTIVE`\.

1. The backend service calls `DescribeGameSessionPlacement()` to detect the updated ticket status\. The backend service then uses the connection information to connect the game client to the server process and join the game session\.

## Add a player to a game<a name="gamelift-sdk-interactions-add-player"></a>

This sequence describes the process of adding a player to an existing game session\. Player sessions can also be requested as part of a game session placement request\.

1. The backend service calls the client API operation `CreatePlayerSession()` with a game session ID\.

1. GameLift checks the game session status \(must be `ACTIVE`\), and looks for an open player slot in the game session\. If a slot is available, then GameLift does the following:

   1. Creates a new `PlayerSession` object and sets the status to `RESERVED`\.

   1. Responds to the backend service request with the `PlayerSession` object\.

1. The backend service connects the game client directly to the server process with the player session ID\.

1. The server calls the server API operation `AcceptPlayerSession()` to validate the player session ID\. If validated, then GameLift passes the `PlayerSession` object to the server process\. The server process either accepts or rejects the connection\.

1. GameLift does one of the following:

   1. If the connection is accepted, then GameLift sets the `PlayerSession` status to `ACTIVE`\.

   1. If no response is received within 60 seconds of the backend server's original `CreatePlayerSession()` call, then GameLift changes the `PlayerSession` status to `TIMEDOUT` and reopens the player slot in the game session\.

## Remove a player<a name="gamelift-sdk-interactions-remove-player"></a>

When removing players from a game session to create space for new players to join, the following interactions occur\.

1. A player disconnects from the game\.

1. The server detects the lost connection and calls the server API operation `RemovePlayerSession()`\.

1. GameLift changes the `PlayerSession` status to `COMPLETED` and reopens the player slot in the game session\.

## Shut down the game session<a name="gamelift-sdk-interactions-shutdown"></a>

This sequence of interactions occurs when a server process shuts down the current game session\. 

1. The server shuts down the game session and server\.

1. The server calls `ProcessEnding()` to GameLift\.

1. GameLift does the following:

   1. Uploads game session logs to Amazon Simple Storage Service \(Amazon S3\)\.

   1. Changes the `GameSession` status to `TERMINATED`\.

   1. Changes the server process status to `TERMINATED`\.

   1. Recycles instance resources\.