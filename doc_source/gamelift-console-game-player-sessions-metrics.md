# View Data on Game and Player Sessions<a name="gamelift-console-game-player-sessions-metrics"></a>

You can view information about the games running on a fleet and as well as individual players\. For more information about game sessions and player sessions, see [How Players Connect to Games](game-sessions-intro.md)\. 

**To view game session data**

1. In the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/), open the detail page for the fleet you want to study\. \(Choose **Fleets** in the menu bar and click on a fleet name\.\)

1. Open the **Game sessions **tab\. This tab lists all game sessions hosted on the fleet along with summary information\.

1. Click a game session to view additional information about the game session as well as a list of players that were connected to the game\. 

## Game sessions<a name="game-sessions"></a>

A summary of your game session information is displayed at the top of the page and includes:
+ **Status** – Game session status\. Valid statuses are:
  + **Activating** – A game session has been initiated and is preparing to run\.
  + **Active** – A game session is running and available to receive players \(depending on the session's [player creation policy](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\)\.
  + **Terminated** – Game session has ended\.
+ **Name** – Game generated for the game session\.
+ **ID** – Unique identifier assigned by Amazon GameLift to the game session\.
+ **IP address** – IP address specified for the game session\.
+ **Port ** – Port number used to connect to the game session\.
+ **Player sessions** – Number of players connected to the game sessions along with total possible players the game session can support\. For example: 2 \(connected players\) of 10 \(possible players\) means the fleet can support 8 additional players\. 
+ **Uptime** – Total length of time the game session has been running\.
+ **Date created** – Date and time stamp indicating when the fleet was created\.

## Player sessions<a name="player-sessions"></a>

The following player session data is collected for each game session:
+ **Status** – The status of the player session\. Options include:
  + **Reserved** – Player session has been reserved, but the player has not yet connected\.
  + **Active** – Player session is currently connected to the game server\.
  + **Completed** – Player session has ended; player is no longer connected\.
  + **Timed Out** – Player session was reserved, but the player failed to connect\.
+ **ID** – The identifier assigned to the player session\.
+ **Player ID** – A unique identifier for the player\. Click this ID to get additional player information\.
+ **Start time** – The time the player connected to the game session\.
+ **End time** – The time the player disconnected from the game session\.
+ **Total time** – The total length of time the player has been active in the player session\.

## Player information<a name="player-info"></a>

View additional information for a selected player, including a list of all games the player connected to across all fleets in the current region\. This information includes the status, start and end times, and total connected time for each player session\. You can click to view data for the relevant game sessions and fleets\. 