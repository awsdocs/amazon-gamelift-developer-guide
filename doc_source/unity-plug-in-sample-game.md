# Importing and running a sample game<a name="unity-plug-in-sample-game"></a>

The Amazon GameLift Plug\-in for Unity includes a sample game you can use to explore the basics of integrating your game with Amazon GameLift\. In this section, you build the game client and game server and then test locally using GameLift Local\. You will see GameLift messages from the game server and game client in the log window\. 

**To build and run the sample game client**

1. In Unity, on the menu, select **GameLift**, and then choose **Import Sample Game**\.

1. In the **Import Sample Game** window, choose **Import** to import the game and all of its assets and dependencies\.

1. Build the game server\. In Unity, on the menu, select **GameLift**, and then choose **Apply Sample Server Build Settings**\. After the game server settings are configured, Unity will recompile assets\. 

1. In Unity, on the menu, select **File**, and then choose **Build Settings\.\.\.**, confirm **Server Build** is checked, choose **Build**, and then select a build folder\. 

   Unity will build the sample game server, placing the executable and required assets in the specified build folder\.

1. Close the build window\.

1. In Unity, on the menu, select **GameLift**, and then choose **Apply Sample Client Build Settings**\. After the game client settings are configured, Unity will recompile assets\. 

1. In Unity, on the menu, select **Go To Client Settings**\. This will display an **Inspector** tab on the right side of the Unity screen\. In the **GameLift Client Settings** tab, choose **Local Testing Mode**\. 

1. Build the game client\. In Unity, on the menu, select **File**, and then choose **Build Settings\.\.\.**, confirm **Server Build** is not checked, choose **Build**, and then select a build folder\. 

   Unity will build the sample game client, placing the executable and required assets in the specified build folder\.

1. Close the build window\.

   The game server and client are built\. In the next few steps, you run the game and see how it interacts with GameLift\.

1. In Unity, in the Plug\-in for Unity tab, select the **Deploy** tab\.

1. In the **Test** pane, select **Open Local Test UI**\. 

1. In the **Local Testing** window, specify a **Game Server \.exe File Path**\. The path must include the executable name\. For example, `C:/MyGame/GameServer/MyGameServer.exe`\. 

1. Select **Deploy and Run**\. The Plug\-in for Unity will launch the game server and open a GameLift Local log window\. The windows will contain log messages including messages sent between the game server and GameLift Local\. 

1. Launch the game client\. You can find it in the build location you specified when building the sample game client\.

1. In the **GameLift Sample Game**, provide an email and password and then select **Log In**\. The email and password are not validated and are not used\.

   

1. In the **GameLift Sample Game**, choose **Start**\. The game client will look for a game session\. If one cannot be found, it will create a game session\. The game client then starts the game session\. You can see game activity in the logs\.

   The messages below are from the sample game server:

   ```
   ...
   2021-09-15T19:55:3495 PID:20728 Log :) GAMELIFT AWAKE 
   2021-09-15T19:55:3512 PID:20728 Log :) I AM SERVER 
   2021-09-15T19:55:3514 PID:20728 Log :) GAMELIFT StartServer at port 33430. 
   2021-09-15T19:55:3514 PID:20728 Log :) SDK VERSION: 4.0.2 
   2021-09-15T19:55:3556 PID:20728 Log :) SERVER IS IN A GAMELIFT FLEET 
   2021-09-15T19:55:3577 PID:20728 Log :) PROCESSREADY SUCCESS. 
   2021-09-15T19:55:3577 PID:20728 Log :) GAMELIFT HEALTH CHECK REQUESTED (HEALTHY)
   ...
   2021-09-15T19:55:3634 PID:20728 Log :) GAMELOGIC AWAKE 
   2021-09-15T19:55:3635 PID:20728 Log :) GAMELOGIC START 
   2021-09-15T19:55:3636 PID:20728 Log :) LISTENING ON PORT 33430 
   2021-09-15T19:55:3636 PID:20728 Log SERVER: Frame: 0 HELLO WORLD! 
   ...
   2021-09-15T19:56:2464 PID:20728 Log :) GAMELIFT SESSION REQUESTED
   2021-09-15T19:56:2468 PID:20728 Log :) GAME SESSION ACTIVATED
   2021-09-15T19:56:3578 PID:20728 Log :) GAMELIFT HEALTH CHECK REQUESTED (HEALTHY)
   2021-09-15T19:57:3584 PID:20728 Log :) GAMELIFT HEALTH CHECK REQUESTED (HEALTHY)
   2021-09-15T19:58:0334 PID:20728 Log SERVER: Frame: 8695 Connection accepted: playerIdx 0 joined
   2021-09-15T19:58:0335 PID:20728 Log SERVER: Frame: 8696 Connection accepted: playerIdx 1 joined 
   2021-09-15T19:58:0338 PID:20728 Log SERVER: Frame: 8697 Msg rcvd from playerIdx 0 Msg: CONNECT: server IP localhost 
   2021-09-15T19:58:0338 PID:20728 Log SERVER: Frame: 8697 Msg rcvd from player 0:CONNECT: server IP localhost 
   2021-09-15T19:58:0339 PID:20728 Log SERVER: Frame: 8697 CONNECT: player index 0 
   2021-09-15T19:58:0339 PID:20728 Log SERVER: Frame: 8697 Msg rcvd from playerIdx 1 Msg: CONNECT: server IP localhost 
   2021-09-15T19:58:0339 PID:20728 Log SERVER: Frame: 8697 Msg rcvd from player 1:CONNECT: server IP localhost 
   2021-09-15T19:58:0339 PID:20728 Log SERVER: Frame: 8697 CONNECT: player index 1
   ```

   The messages below are messages from GameLift Local:

   ```
   12:55:26,000  INFO || - [SocketIOServer] main - Session store / pubsub factory used: MemoryStoreFactory (local session store only)
   12:55:28,092  WARN || - [ServerBootstrap] main - Unknown channel option 'SO_LINGER' for channel '[id: 0xe23d0a14]'
   12:55:28,101  INFO || - [SocketIOServer] nioEventLoopGroup-2-1 - SocketIO server started at port: 5757
   12:55:28,101  INFO || - [SDKConnection] main - GameLift SDK server (communicates with your game server) has started on http://localhost:5757
   12:55:28,120  INFO || - [SdkWebSocketServer] WebSocketSelector-20 - WebSocket Server started on address localhost/127.0.0.1:5759
   12:55:28,166  INFO || - [StandAloneServer] main - GameLift Client server (listens for GameLift client APIs) has started on http://localhost:8080
   12:55:28,179  INFO || - [StandAloneServer] main - GameLift server sdk http listener has started on http://localhost:5758
   12:55:35,453  INFO || - [SdkWebSocketServer] WebSocketWorker-12 - onOpen socket: /?pID=20728&sdkVersion=4.0.2&sdkLanguage=CSharp and handshake /?pID=20728&sdkVersion=4.0.2&sdkLanguage=CSharp
   12:55:35,551  INFO || - [HostProcessManager] WebSocketWorker-12 - client connected with pID 20728
   12:55:35,718  INFO || - [GameLiftSdkHttpHandler] GameLiftSdkHttpHandler-thread-0 - GameLift API to use: ProcessReady for pId 20728
   12:55:35,718  INFO || - [ProcessReadyHandler] GameLiftSdkHttpHandler-thread-0 - Received API call for processReady from 20728
   12:55:35,738  INFO || - [ProcessReadyHandler] GameLiftSdkHttpHandler-thread-0 - onProcessReady data: port: 33430
    12:55:35,739  INFO || - [HostProcessManager] GameLiftSdkHttpHandler-thread-0 - Registered new process with pId 20728
   12:55:35,789  INFO || - [GameLiftSdkHttpHandler] GameLiftSdkHttpHandler-thread-0 - GameLift API to use: ReportHealth for pId 20728
   12:55:35,790  INFO || - [ReportHealthHandler] GameLiftSdkHttpHandler-thread-0 - Received API call for ReportHealth from 20728
   12:55:35,794  INFO || - [ReportHealthHandler] GameLiftSdkHttpHandler-thread-0 - ReportHealth data: healthStatus: true
    12:56:24,098  INFO || - [GameLiftHttpHandler] Thread-12 - API to use: GameLift.DescribeGameSessions
   12:56:24,119  INFO || - [DescribeGameSessionsDispatcher] Thread-12 - Received API call to describe game sessions with input: {"FleetId":"fleet-123"}
   12:56:24,241  INFO || - [GameLiftHttpHandler] Thread-12 - API to use: GameLift.CreateGameSession
   12:56:24,242  INFO || - [CreateGameSessionDispatcher] Thread-12 - Received API call to create game session with input: {"FleetId":"fleet-123","MaximumPlayerSessionCount":4}
   12:56:24,265  INFO || - [HostProcessManager] Thread-12 - Reserved process: 20728 for gameSession: arn:aws:gamelift:local::gamesession/fleet-123/gsess-59f6cc44-4361-42f5-95b5-fdb5825c0f3d
   12:56:24,266  INFO || - [WebSocketInvoker] Thread-12 - StartGameSessionRequest: gameSessionId=arn:aws:gamelift:local::gamesession/fleet-123/gsess-59f6cc44-4361-42f5-95b5-fdb5825c0f3d, fleetId=fleet-123, gameSessionName=null, maxPlayers=4, properties=[], ipAddress=127.0.0.1, port=33430, gameSessionData?=false, matchmakerData?=false, dnsName=localhost
   12:56:24,564  INFO || - [CreateGameSessionDispatcher] Thread-12 - GameSession with id: arn:aws:gamelift:local::gamesession/fleet-123/gsess-59f6cc44-4361-42f5-95b5-fdb5825c0f3d created
   12:56:24,585  INFO || - [GameLiftHttpHandler] Thread-12 - API to use: GameLift.DescribeGameSessions
   12:56:24,585  INFO || - [DescribeGameSessionsDispatcher] Thread-12 - Received API call to describe game sessions with input: {"FleetId":"fleet-123"}
   12:56:24,660  INFO || - [GameLiftSdkHttpHandler] GameLiftSdkHttpHandler-thread-0 - GameLift API to use: GameSessionActivate for pId 20728
   12:56:24,661  INFO || - [GameSessionActivateHandler] GameLiftSdkHttpHandler-thread-0 - Received API call for GameSessionActivate from 20728
   12:56:24,678  INFO || - [GameSessionActivateHandler] GameLiftSdkHttpHandler-thread-0 - GameSessionActivate data: gameSessionId: "arn:aws:gamelift:local::gamesession/fleet-123/gsess-59f6cc44-4361-42f5-95b5-fdb5825c0f3d"
   ```

1. In the game client, choose **Quit** or close the window to stop the game client\. 

1. In Unity, in the **Local Testing** window, choose **Stop** or close the game server windows to stop the server\. 