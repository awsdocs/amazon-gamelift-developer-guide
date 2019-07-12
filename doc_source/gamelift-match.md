# How Amazon GameLift FlexMatch Works<a name="gamelift-match"></a>

This topic provides an overview of FlexMatch matchmaking, including key features, components, and how the matchmaking process works\. For detailed help with adding FlexMatch to your game, including how to set up a matchmaker and customize player matching, see [Adding FlexMatch Matchmaking](match-intro.md)\. 

Amazon GameLift FlexMatch is a customizable matchmaking service\. It offers flexible tools that let you manage the full matchmaking experience in a way that best fits your game\. With FlexMatch, you can build teams for your game matches, select compatible players, and find the best available hosting resources for an optimum player experience\. You can also use FlexMatch backfill to find new players for existing games, so that games stay filled with compatible players throughout the life of the game session, for the best possible player experience\.

With FlexMatch you can create and run as many matchmakers to fit your game modes and your players\. For example, you would likely have different matchmakers to build teams for a free\-for\-all and a cage match\. 

## FlexMatch Key Features<a name="gamelift-match-features"></a>
+ **Customize player matching\.** Design and build the types of multiplayer experiences that your players will find most compelling\. For each game mode, define the team structure and set other game attributes\. Build a set of custom rules to evaluate player attributes \(such as skill level or role\) and form the best possible player matches for a game\. Use these rules to group players for new matches or find players to fill open slots in existing matches \("match backfill"\)\. 
+ **Create large matches\.** FlexMatch can be used to create very large matches\-\-between 41 and 200 players\-\-using a large\-match algorithm that streamlines the matching process\. When creating large matches, you can choose between placing a higher priority on creating larger matches with more similar players or creating matches where all players have the best possible player latency experience\.
+ **Get player acceptance\.** Require all players to accept a proposed match before starting\. If this feature is enabled, FlexMatch waits for all players assigned to a match to accept it before the match begins\.

  **Support player parties\.**Generate matches for a group of players who want to play together on the same team\. Find additional players to fill out the match as needed\.
+ **Match players based on latency\.**Use player latency information to ensure that matched players have similar response times\. This feature avoids disparities in gameplay that might give some players undue advantage\. It is particularly valuable when creating matches that span multiple geographic areas\.
+ **Relax player matching rules over time\.** Strike the right balance between creating the best possible player matches and getting players into good matches fast\. You decide where and when to relax strict matching rules into order to get players into games with minimal wait time\. 
+ **Find the best hosting resources\.** Use game and player information to select the best available resources to host the match for optimal gameplay experience\.
+ **Keep games filled with matched players\.** Use FlexMatch backfill to fill empty player slots with well\-matched new players throughout the life span of the game session\. You can opt to enable automatic backfill or add code to your game to manage backfill manually\.

## FlexMatch Components<a name="gamelift-match-components"></a>

Amazon GameLift FlexMatch requires these three key components to work together:
+ **Mechanisms to trigger player matchmaking\.** One mechanism determines when to initiate matchmaking for players\. A second \(optional\) mechanism determines when to find new players for empty slots in an existing match \(backfilling\)\. Matchmaking and match backfill requests are handed to a matchmaker for processing\.
+ **FlexMatch matchmaker to evaluate players and create matches\.** A matchmaker builds the best possible player matches from the requests it receives\. It has a rule set that defines a match's team structure and sets the criteria to use when evaluating players for a match\. A game can have multiple matchmakers, with each building a different type of match\. 
+ **Game session queue to place new matches\.** A game session queue finds available computing resources to host a match\. It determines where \(in what regions\) to look for resources and how to select the best available host for each match\. 

The following sections detail how matchmaking proceeds to form new game matches or to find new players for existing game matches\. 

## Matchmaking Process<a name="gamelift-match-howitworks"></a>

Here's how requests for a new game match are handled with FlexMatch\. This description assumes that a client\-side game service is initiating matchmaking requests and tracking the matchmaking ticket status\.

1. **Request matchmaking\. **Players take some action in your game that triggers matchmaking, such as clicking a "Join Now" button or a group of players forming a party\. Your game initiates a matchmaking request, identifying which matchmaker to use and including one or more players to be matched\. The request includes any player information, such as skill level or preferences, that the matchmaker requires to build matches\. Each request gets a matchmaking ticket ID, which your game uses to track the request status and take action as needed\. 

1. **Discover potential matches\.** All matchmaking tickets are passed to the specified matchmaker and placed in its ticket pool for processing\. A ticket remains in a ticket pool until it is matched or until it reaches the matchmaker's maximum time limit\.

   To find player matches for a regular \(non\-large\) match, the matchmaker makes continual passes through the ticket pool\. On each pass, the matchmaker starts with the oldest ticket in the pool and evaluates the other tickets against it to find the best possible matches\. A matchmaker's rule set determines \(1\) how many teams to create for a match, \(2\) the number of players to assign to each team, and \(3\) how to evaluate each prospective player\. Rules might set requirements for individual players, teams, or matches\. For example, a rule might require all matched players to have a certain talent, or it might require at least one player on a team to play a certain character\. A commonly used rule requires that all players in a match have similar skill ratings\. 

   For large matches, the process is slightly different\. Instead of evaluating each player against a set of rules, FlexMatch weighs available matchmaking tickets against a single key balancing attribute and groups players that have similar attribute values\. It also applies latency requirements\. You can choose between either creating larger matches with greater similarity between players, or placing players into matches that give them the best possible latency experience\. Once an initial match is found, FlexMatch then does a series of tests to ensure that the final match is the best available solution\.

   When the matchmaker evaluates a ticket, it either passes or fails the entire ticket\. For tickets with multiple players, the matchmaker assumes these players want to play together and attempts to place them all in the same match\. This means that, for any potential match, all the players in a ticket must be acceptable\. If any player fails any rule, the entire ticket is considered not a match\. Tickets that fail remain in the ticket pool and are evaluated again on the next pass\. Once a potential match is filled, the status of all tickets in the match are updated\.

1. **Get player acceptance\. **If the matchmaker requires players to accept a potential match, FlexMatch cannot proceed with the match until every player accepts\. The matchmaking ticket status is changed to indicate that acceptance is required, which prompts your game to request acceptances from all players in each matched ticket\.

   Players can choose to accept or reject a potential match\. Your game collects the player responses and reports them back to FlexMatch\. All players in the potential match must accept the match within a certain time limit to continue\. If any player rejects the match or fails to respond before the time limit, the matchmaker drops the potential match\. Tickets for players who accepted the match are returned to the matchmaker's ticket pool; tickets for players who did not accept the match move to a failure status and are no longer processed\.

1. **Find resources to host the match\. **Once a potential match is made and accepted, FlexMatch tries to place the match with available hosting resources\. The matchmaker is configured to use a specific game session queue, and it passes the potential match to that queue for placement\. The queue uses a set of rules to search regions and fleets for the best available server process to host the match\. If the original matchmaking request contained player latency data, the queue uses this information to find resources that offer the lowest latency and most consistent gameplay experience for players in the match\. 

   Once an available server process is located, Amazon GameLift creates a game session record with game properties and matchmaker data, including team structure and sizes, player assignments, and relevant player characteristics\. 

1. **Start a new game session\.** As when starting any new game sessions, Amazon GameLift sends a start request to the server process along with the game session and matchmaker information\. The server process takes the information and uses it to start a new game session for a matched game\. When the game session is ready to accept players, the server process notifies Amazon GameLift\. 

1. **Connect players to the new game session\.** Once the game session is ready for players, Amazon GameLift creates new player sessions for every player in the match\. It then updates all matchmaking tickets, changing the ticket status to indicate success and adding connection information for all players\. This change in ticket status, prompts your game to relay the connection information to game clients\. Players can now join the game and claim their slots in the match and their team assignments\.

## Backfill Process<a name="gamelift-match-howitworks-backfill"></a>

Here's how finding new players for an existing match is handled with FlexMatch\. Since match backfill requires up\-to\-date information on player slot availability in game sessions, we recommend initiating match backfill requests from the game server\. Another option is to use a client\-side game service, such as a session directory service, that tracks game session and player activity\. See more on adding the match backfill feature to your game at [Backfill Existing Games with FlexMatch](match-backfill.md)\.

If a matchmaking configuration has automatic backfill enabled, the process is similar\. The only difference is that the initial backfill request is generated by GameLift instead of by your code\. Automatic backfill requests are triggered when a game session has an open player slot\.

1. **Request backfill matchmaking\.** A matched game has empty player slots that need to be filled\. Your game initiates a backfill request, identifying which matchmaker to use and describing the current players in the game session\. Each request has a matchmaking ticket ID, which your game uses to track the request status and take action as needed\. With automatic backfill, this ticket ID is added to the game session's matchmaking data\.

1. **Discover potential matches\.** Matchmaking tickets for backfills are passed to the specified matchmaker and placed in the same pool as tickets for new matches\. For large matches only, backfill tickets are prioritized over tickets for new matches\. 

   The matchmaker evaluates tickets and players equally, whether the tickets are for new players or a backfill request\. The one exception is that a potential match cannot have more than one backfill ticket\. A backfill ticket must be matched with at least one other ticket in order to complete successfully, even when the matchmaker's rules allow a match to complete with empty player slots\. Once a potential match is filled, the status of all tickets in the match is updated\.

1. **Get player acceptance\.** If acceptance is required, only the new players need to accept a backfill match, and this step is handled as described for matchmaking requests\. The current players do not need to accept a match that they're already playing in\. As a result, even though the backfill request's ticket status indicates that acceptance is required, your game does not need to take action\. 

   If any of the proposed new players fails to accept the match within the time limit, the potential match is dropped and no new players are added to the existing match\. When this happens, the ticket for the backfill request returns to the ticket pool for processing\.

1. **Update existing game session with new match data\.** When a backfill match is successfully made there is no need to place a new game session\. Instead, Amazon GameLift updates the match data for the existing game session, adding the new players and team assignments\. Amazon GameLift sends the updated game session information to the server process that is hosting the existing game\. 

1. **Connect new players to the existing game session\.** Amazon GameLift creates player sessions for the new players and updates the matchmaking tickets with current status, player sessions, and connection information\. Your client game service, which is tracking ticket status of the new players, relays the connection information to the game clients\. Players can now join the existing game and claim their player slot\.