# View data on game and player sessions<a name="gamelift-console-game-player-sessions-metrics"></a>

You can view information about the games sessions and individual players\. For more information about game sessions and player sessions, see [How players connect to games](game-sessions-intro.md)\. 

**To view game session and player data**

1. In the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Fleets**\.

1. Choose the fleet from the **Fleets** list that hosted your game sessions\.

1. Choose the **Game sessions** tab\. This tab lists all game sessions hosted on the fleet along with summary information\.

1. Choose a game session to view additional information about the game session and a list of players that connected to the game\. 

## Details<a name="game-sessions"></a>

**Overview**  
This section displays a summary of your game session information\.
+ **Status** – Game session status\.
  + **Activating** – The instance is initiating a game session\.
  + **Active** – A game session is running and available to receive players, depending on the session's [player creation policy](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\.
  + **Terminated** – the game session has ended\.
+ **ARN** – The Amazon Resource Name of the game session\.
+ **Name** – Name generated for the game session\.
+ **Location** – The location that GameLift hosted the game session in\.
+ **Creation time** – Date and time that GameLift created the stream session\.
+ **Ending time** – Date and time that the game session ended\.
+ **DNS name** – The host name of the game session\.
+ **IP address** – IP address specified for the game session\.
+ **Port ** – Port number used to connect to the game session\.
+ **Creator ID** – A unique identifier of the player that initiated the game session\.
+ **Player session creation policy** – Indicates if the game session is accepting new players\.
+ **Game scaling protection policy** – The type of game session protection to set on all new instances that GameLift starts in the fleet\.

**Game data**  
Well\-formatted data to send to your game session on start\.

**Game properties**  
Key and value pair properties that influence your game session\.

**Matchmaking data**  
The FlexMatch matchmaker JSON\. To review and edit the matchmaker choose **View matchmaking configuration**\. For more information about FlexMatch matchmaking, see [Build a matchmaker](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/matchmaker-build.html)\.

## Player sessions<a name="player-sessions"></a>

The following player session data is collected for each game session:
+ **Player session ID** – The identifier assigned to the player session\.
+ **Player ID** – A unique identifier for the player\. Choose this ID to get additional player information\.
+ **Status** – The status of the player session\. The following are possible statuses:
  + **Reserved** – Player session has been reserved, but the players isn't connected\.
  + **Active** – Player session is connected to the game server\.
  + **Completed** – Player session has ended; player is no longer connected\.
  + **Timed Out** – Player failed to connect\.
+ **Creation time** – The time the player connected to the game session\.
+ **Ending time** – The time the player disconnected from the game session\.
+ **Player data** – Information about the player provided during player session creation\.

## Player information<a name="player-info"></a>

View additional information for a selected player, including a list of all games the player connected to across all fleets in the current region\. This information includes the status, start times, end times, and total connected time for each player session\. You can choose to view data for the relevant game sessions and fleets\. 