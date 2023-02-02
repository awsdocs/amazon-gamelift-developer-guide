# Add Amazon GameLift to your Unity game server project<a name="integration-unity-server"></a>

This topic helps you prepare your custom game server for hosting on GameLift\. The game server must be able to notify GameLift about its status, to start and stop game sessions when prompted, and to perform other tasks\. For more information, see  [Add GameLift to your game server](gamelift-sdk-server-api.md)\.

## Prerequisites<a name="integration-unity-server-prereq"></a>

Before integrating your game server, complete the following tasks: 
+ [Set up an IAM service role for GameLift](setting-up-role.md)
+ [Installing and setting up the plug\-in for Unity](unity-plug-in-install.md)

## Set up a new server process<a name="server-process"></a>

Set up communication with GameLift and report that the server process is ready to host a game session\. 

1. Initialize the server SDK by calling `InitSDK()`\. 

1. To prepare the server to accept a game session, call `ProcessReady()` with the connection port and game session location details\. Include the names of callback functions that GameLift service invokes, such as `OnGameSession()`, `OnGameSessionUpdate()`, `OnProcessTerminate()`, `OnHealthCheck()`\. GameLift might take a few minutes to provide a callback\.

1. GameLift updates the status of the server process to `ACTIVE`\.

1. GameLift calls `onHealthCheck` periodically\.

The following code example shows how to set up a simple server process with GameLift\. 

```
//initSDK
var initSDKOutcome = GameLiftServerAPI.InitSDK();
           
//processReady
// Set parameters and call ProcessReady
var processParams = new ProcessParameters(
    this.OnGameSession,
    this.OnProcessTerminate,
    this.OnHealthCheck,
    this.OnGameSessionUpdate,
    port,
    // Examples of log and error files written by the game server
    new LogParameters(new List<string>()          
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
}
               
bool OnHealthCheck()
{
    bool isHealthy;
    // complete health evaluation within 60 seconds and set health
    return isHealthy;
}
```

## Start a game session<a name="start-game-session"></a>

After game initialization is complete, you can start a game session\.

1. Implement the callback function `onStartGameSession`\. GameLift invokes this method to start a new game session on the server process and receive player connections\.

1. To activate a game session, call `ActivateGameSession()`\. For more information about the SDK, see [GameLift server API \(C\#\) reference: Actions](integration-server-sdk-csharp-ref-actions.md)\.

The following code example illustrates how to start a game session with GameLift\. 

```
void OnStartGameSession(GameSession gameSession)
{
    // game-specific tasks when starting a new game session, such as loading map   
    ...
    // When ready to receive players   
    var activateGameSessionOutcome = GameLiftServerAPI.ActivateGameSession();
}
```

## End a game session<a name="end-game-session"></a>

Notify GameLift when a game session is ending\. As a best practice, shut down server processes after game sessions complete to recycle and refresh hosting resources\. 

1. Set up a function named `onProcessTerminate` to receive requests from GameLift and call `ProcessEnding()`\. 

1. The process status changes to `TERMINATED`\.

The following example describes how to end a process for a game session\.

```
var processEndingOutcome = GameLiftServerAPI.ProcessEnding();

if (processReadyOutcome.Success)
   Environment.Exit(0);

// otherwise, exit with error code
Environment.Exit(errorCode);
```

## Create server build and upload to GameLift<a name="gamelift-connection"></a>

After you integrate your game server with GameLift, upload the build files to a fleet so that GameLift can deploy it for game hosting\. For more information on how to upload your server to GameLift, see [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md)\.