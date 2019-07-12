# Backfill Existing Games with FlexMatch<a name="match-backfill"></a>

Match backfill uses your FlexMatch mechanisms to match new players to existing matched game sessions\. Although you can always add players to any game \(see [Join a Player to a Game Session](gamelift-sdk-client-api.md#gamelift-sdk-client-api-join)\), match backfill ensures that new players meet the same match criteria as current players\. In addition, match backfill assigns the new players to teams, manages player acceptance, and sends updated match information to the game server\. Learn more about match backfill in [Matchmaking Process](gamelift-match.md#gamelift-match-howitworks)\.

**Note**  
FlexMatch backfill is not currently available for games using Realtime Servers\.

To add match backfill to your game, enable automatic backfill or manage backfill requests manually by adding code to your game server or game client\.

## Turn on Automatic Backfill<a name="match-backfill-auto"></a>

With automatic match backfill, GameLift automatically triggers a backfill request whenever a game session has an open player slot\. To add automatic backfill to your game, make the following updates to your game\.

1. **Enable automatic backfill\.** Automatic backfill is managed in a matchmaking configuration\. When enabled, it is used with all matched game sessions that are created with that matchmaker\. GameLift begins generating backfill requests for a non\-full game session as soon as the game session starts up on a game server\.

   To turn on automatic backfill, open a match configuration and set the backfill mode to "AUTOMATIC"\. For more details, see [Create a Matchmaking Configuration](match-create-configuration.md)

1. **Update game session with new matchmaker data\.** Amazon GameLift updates your game server with match information using the Server SDK callback function `onUpdateGameSession` \(see [Prepare a Server Process](gamelift-sdk-server-api.md#gamelift-sdk-server-initialize)\)\. Add code to your game server to handle updated game session objects as a result of backfill activity\. Learn more in [Update Match Data on the Game Server](#match-backfill-server-data)\. 

1. **Turn off automatic backfill for a game session\.** You can opt to stop automatic backfill at any point during an individual game session\. For example, you might want backfill to occur only up to when gameplay actually starts or stop backfilling in the last few minutes of a game\.

   To stop automatic backfill, add code to your game client or game server to make the GameLift API call [StopMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html)\. This call requires a ticket ID\. Use the backfill ticket ID from the latest backfill request\. You can get this information from the game session matchmaking data, which is updated as described in the previous step\.

## Send Backfill Requests \(From a Game Server\)<a name="match-backfill-server"></a>

You can initiate match backfill requests directly from the game server process that is hosting the game session\. The server process has the most up\-to\-date information on current players connected to the game and the status of empty player slots\.

This topic assumes that you've already built the necessary FlexMatch components and successfully added matchmaking processes to your game server and a client\-side game service\. For more details on setting up FlexMatch, see [FlexMatch Integration Roadmap](match-tasks.md)\. 

To enable match backfill for your game, add the following functionality:
+ Send matchmaking backfill requests to a matchmaker and track the status of requests\.
+ Update match information for the game session\. See [ Update Match Data on the Game Server  No matter how you initiate match backfill requests in your game, your game server must be able to handle the game session updates that Amazon GameLift delivers as a result of match backfill requests\. When Amazon GameLift completes a match backfill request—successfully or not—it calls your game server using the callback function `onUpdateGameSession`\. This call has three input parameters: a match backfill ticket ID, a status message , and a GameSession object containing the most up\-to\-date matchmaking data including player information\. You need to add the following code to your game server as part of your game server integration:    Implement the `onUpdateGameSession` function\. This function must be able to handle the following status messages \(`updateReason`\):    MATCHMAKING\_DATA\_UPDATED – New players were successfully matched to the game session\. The `GameSession` object contains updated matchmaker data, including player data on existing players and newly matched players\.    BACKFILL\_FAILED – The match backfill attempt failed due to an internal error\. The `GameSession` object is unchanged\.   BACKFILL\_TIMED\_OUT – The matchmaker failed to find a backfill match within the time limit\. The `GameSession` object is unchanged\.   BACKFILL\_CANCELLED – The match backfill request was canceled by a call to StopMatchmaking \(client\) or StopMatchBackfill \(server\)\. The `GameSession` object is unchanged\.     For successful backfill matches, use the updated matchmaker data to handle the new players when they connect to the game session\. At a minimum, you'll need to use the team assignments for the new player\(s\), as well as other player attributes that are required to get the player started in the game\.   In your game server's call to the Server SDK action [ProcessReady\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processready), add the `onUpdateGameSession` callback method name as a process parameter\.   ](#match-backfill-server-data)

As with other server functionality, a game server uses the Amazon GameLift Server SDK\. This SDK is available in C\+\+ and C\#\. For a general description of server APIs, see the [Server API references](reference-intro.md)\.

To make match backfill requests from your game server, complete the following tasks\.

1. **Trigger a match backfill request\.** Generally, you want to initiate a backfill request whenever a matched game has one or more empty player slots\. You may want to tie backfill requests to specific circumstances, such as to fill critical character roles or balance out teams\. You'll likely also want to limit backfilling activity based on a game session's age\. 

1. **Create a backfill request\.** Add code to create and send match backfill requests to a FlexMatch matchmaker\. Backfill requests are handled using these server APIs:
   + [StartMatchBackfill\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-startmatchbackfill)
   + [StopMatchBackfill\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-stopmatchbackfill)

   To create a backfill request, call `StartMatchBackfill` with the following information\. To cancel a backfill request, call `StopMatchBackfill` with the backfill request's ticket ID\.
   + **Ticket ID** — Provide a matchmaking ticket ID \(or opt to have them autogenerated\)\. You can use the same mechanism to assign ticket IDs to both matchmaking and backfill requests\. Tickets for matchmaking and backfilling are processed the same way\.
   + **Matchmaker** — Identify which matchmaker to use for the backfill request\. Generally, you'll want to use the same matchmaker that was used to create the original match\. This request takes a matchmaking configuration ARN\. This information is stored in the game session object \([GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\), which was provided to the server process by Amazon GameLift when activating the game session\. The matchmaking configuration ARN is included in the `MatchmakerData` property\. 
   + **Game session ARN** — Identify the game session being backfilled\. You can get the game session ARN by calling the server API [GetGameSessionId\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-getgamesessionid)\. During the matchmaking process, tickets for new requests do not have a game session ID, while tickets for backfill requests do\. The presence of at game session ID is one way to tell the difference between tickets for new matches and tickets for backfills\.
   + **Player data** — Include player information \([Player](https://docs.aws.amazon.com/gamelift/latest/apireference/API_Player.html)\) for all current players in the game session you are backfilling\. This information allows the matchmaker to locate the best possible player matches for the players currently in the game session\. If your game server has been accurately reporting player connection status, you should be able to acquire this data as follows: 

     1. The server process hosting the game session should have the most up\-to\-date information which players are currently connected to the game session\. 

     1. To get player IDs, attributes, and team assignments, pull player data from the game session object \([GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\), `MatchmakerData` property \(see [Work with Matchmaker Data](match-server.md#match-server-data)\. The matchmaker data includes all players who were matched to the game session, so you'll need to pull the player data for only the currently connected players\. 

     1. For player latency, if the matchmaker calls for latency data, collect new latency values from all current players and include it in each `Player` object\. If latency data is omitted and the matchmaker has a latency rule, the request will not be successfully matched\. Backfill requests require latency data only for the region that the game session is currently in\. You can get a game session's region from the `GameSessionId` property of the `GameSession` object; this value is an ARN, which includes the region\. 

1. **Track the status of a backfill request\.** Amazon GameLift updates your game server about the status of backfill requests using the Server SDK callback function `onUpdateGameSession` \(see [Prepare a Server Process](gamelift-sdk-server-api.md#gamelift-sdk-server-initialize)\)\. Add code to handle the status messages—as well as updated game session objects as a result of successful backfill requests—at [Update Match Data on the Game Server](#match-backfill-server-data)\. 

   A matchmaker can process only one match backfill request from a game session at a time\. If you need to cancel a request, call [StopMatchBackfill\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-stopmatchbackfill)\. If you need to change a request, call `StopMatchBackfill` and then submit an updated request\.

## Send Backfill Requests \(From a Client Service\)<a name="match-backfill-client"></a>

As an alternative to sending backfill requests from a game server, you may want to send them from a client\-side game service\. To use this option, the client\-side service must have access to current data on game session activity and player connections; if your game uses a session directory service, this might be a good choice\.

This topic assumes that you've already built the necessary FlexMatch components and successfully added matchmaking processes to your game server and a client\-side game service\. For more details on setting up FlexMatch, see [FlexMatch Integration Roadmap](match-tasks.md)\. 

To enable match backfill for your game, add the following functionality:
+ Send matchmaking backfill requests to a matchmaker and track the status of requests\.
+ Update match information for the game session\. See [ Update Match Data on the Game Server  No matter how you initiate match backfill requests in your game, your game server must be able to handle the game session updates that Amazon GameLift delivers as a result of match backfill requests\. When Amazon GameLift completes a match backfill request—successfully or not—it calls your game server using the callback function `onUpdateGameSession`\. This call has three input parameters: a match backfill ticket ID, a status message , and a GameSession object containing the most up\-to\-date matchmaking data including player information\. You need to add the following code to your game server as part of your game server integration:    Implement the `onUpdateGameSession` function\. This function must be able to handle the following status messages \(`updateReason`\):    MATCHMAKING\_DATA\_UPDATED – New players were successfully matched to the game session\. The `GameSession` object contains updated matchmaker data, including player data on existing players and newly matched players\.    BACKFILL\_FAILED – The match backfill attempt failed due to an internal error\. The `GameSession` object is unchanged\.   BACKFILL\_TIMED\_OUT – The matchmaker failed to find a backfill match within the time limit\. The `GameSession` object is unchanged\.   BACKFILL\_CANCELLED – The match backfill request was canceled by a call to StopMatchmaking \(client\) or StopMatchBackfill \(server\)\. The `GameSession` object is unchanged\.     For successful backfill matches, use the updated matchmaker data to handle the new players when they connect to the game session\. At a minimum, you'll need to use the team assignments for the new player\(s\), as well as other player attributes that are required to get the player started in the game\.   In your game server's call to the Server SDK action [ProcessReady\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processready), add the `onUpdateGameSession` callback method name as a process parameter\.   ](#match-backfill-server-data)

As with other client functionality, a client\-side game service uses the AWS SDK with Amazon GameLift API\. This SDK is available in C\+\+, C\#, and several other languages\. For a general description of client APIs, see the Amazon GameLift Service API Reference, which describes the low\-level service API for Amazon GameLift\-related actions and includes links to language\-specific reference guides\.

To set up a client\-side game service to backfill matched games, complete the following tasks\.

1. **Trigger a request for backfilling\.** Generally, a game initiates a backfill request whenever a matched game has one or more empty player slots\. You may want to tie backfill requests to specific circumstances, such as to fill critical character roles or balance out teams\. You'll likely also want to limit backfilling based on a game session's age\. Whatever you use for a trigger, at a minimum you'll need to the following information\. You can get this information from the game session object \([GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\) by calling [DescribeGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html) with a game session ID\.
   + *Number of currently empty player slots*\. This value can be calculated from a game session's maximum player limit and the current player count\. Current player count is updated whenever your game server contacts the Amazon GameLift service to validate a new player connection or to report a dropped player\.
   + *Creation policy*\. This setting indicates whether the game session is currently accepting new players\.

    The game session object contains other potentially useful information, including game session start time, custom game properties, and matchmaker data\. 

1. **Create a backfill request\.** Add code to create and send match backfill requests to a FlexMatch matchmaker\. Backfill requests are handled using these client APIs:
   + [StartMatchBackfill](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchBackfill.html)
   + [StopMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html)

   To create a backfill request, call `StartMatchBackfill` with the following information\. A backfill request is similar to a matchmaking request \(see [Request Matchmaking for Players](match-client.md#match-client-start)\), but also identifies the existing game session\. To cancel a backfill request, call `StopMatchmaking` with the backfill request's ticket ID\.
   + **Ticket ID** — Provide a matchmaking ticket ID \(or opt to have them autogenerated\)\. You can use the same mechanism to assign ticket IDs to both matchmaking and backfill requests\. Tickets for matchmaking and backfilling are processed the same way\.
   + **Matchmaker** — Identify the name of a matchmaking configuration to use\. Generally, you'll want to use the same matchmaker for backfilling that was used to create the original match\. This information is in a game session object \([GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\), `MatchmakerData` property, under the matchmaking configuration ARN\. The name value is the string following ""matchmakingconfiguration/"\. \(For example, in the ARN value "arn:aws:gamelift:us\-west\-2:111122223333:matchmakingconfiguration/MM\-4v4", the matchmaking configuration name is "MM\-4v4"\.\) 
   + **Game session ARN** — Specify the game session being backfilled\. Use the `GameSessionId` property from the game session object; this ID uses the ARN value that you need\. Matchmaking tickets \([MatchmakingTicket](https://docs.aws.amazon.com/gamelift/latest/apireference/API_MatchmakingTicket.html)\) for backfill requests have the game session ID while being processed; tickets for new matchmaking requests do not get a game session ID until the match is placed; the presence of at game session ID is one way to tell the difference between tickets for new matches and tickets for backfills\.
   + **Player data** — Include player information \([Player](https://docs.aws.amazon.com/gamelift/latest/apireference/API_Player.html)\) for all current players in the game session you are backfilling\. This information allows to matchmaker to locate the best possible player matches for the players currently in the game session\. If your game server has been accurately reporting player connection status, you should be able to acquire this data as follows: 

     1. Call [DescribePlayerSessions\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html) with the game session ID to discover all players who are currently connected to the game session\. Each player session includes a player ID\. You can add a status filter to retrieve active player sessions only\.

     1. Pull player data from the game session object \([GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\), `MatchmakerData` property \(see [Work with Matchmaker Data](match-server.md#match-server-data)\. Use the player IDs acquired in the previous step to get data for currently connected players only\. Since matchmaker data is not updated when players drop out, you will need extract the data for current players only\.

     1. For player latency, if the matchmaker calls for latency data, collect new latency values from all current players and include it in the `Player` object\. If latency data is omitted and the matchmaker has a latency rule, the request will not be successfully matched\. Backfill requests require latency data only for the region that the game session is currently in\. You can get a game session's region from the `GameSessionId` property of the `GameSession` object; this value is an ARN, which includes the region\. 

1. **Track status of backfill request\.** Add code to listen for matchmaking ticket status updates\. You can use the mechanism set up to track tickets for new matchmaking requests \(see [Track Matchmaking Request Status](match-client.md#match-client-track)\) using event notification \(preferred\) or polling\. Although you don't need to trigger player acceptance activity with backfill requests, and player information is updated on the game server, you still need to monitor ticket status to handle request failures and resubmissions\. 

   A matchmaker can process only one match backfill request from a game session at a time\. If you need to cancel a request, call [StopMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html)\. If you need to change a request, call `StopMatchmaking` and then submit an updated request\.

   Once a match backfill request is successful, your game server receives an updated `GameSession` object and handles the tasks needed to join new players to the game session\. See more at [Update Match Data on the Game Server](#match-backfill-server-data)\. 

## Update Match Data on the Game Server<a name="match-backfill-server-data"></a>

No matter how you initiate match backfill requests in your game, your game server must be able to handle the game session updates that Amazon GameLift delivers as a result of match backfill requests\.

When Amazon GameLift completes a match backfill request—successfully or not—it calls your game server using the callback function `onUpdateGameSession`\. This call has three input parameters: a match backfill ticket ID, a status message , and a GameSession object containing the most up\-to\-date matchmaking data including player information\. You need to add the following code to your game server as part of your game server integration: 

1. Implement the `onUpdateGameSession` function\. This function must be able to handle the following status messages \(`updateReason`\): 
   + MATCHMAKING\_DATA\_UPDATED – New players were successfully matched to the game session\. The `GameSession` object contains updated matchmaker data, including player data on existing players and newly matched players\. 
   + BACKFILL\_FAILED – The match backfill attempt failed due to an internal error\. The `GameSession` object is unchanged\.
   + BACKFILL\_TIMED\_OUT – The matchmaker failed to find a backfill match within the time limit\. The `GameSession` object is unchanged\.
   + BACKFILL\_CANCELLED – The match backfill request was canceled by a call to StopMatchmaking \(client\) or StopMatchBackfill \(server\)\. The `GameSession` object is unchanged\.

1. For successful backfill matches, use the updated matchmaker data to handle the new players when they connect to the game session\. At a minimum, you'll need to use the team assignments for the new player\(s\), as well as other player attributes that are required to get the player started in the game\.

1. In your game server's call to the Server SDK action [ProcessReady\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processready), add the `onUpdateGameSession` callback method name as a process parameter\.