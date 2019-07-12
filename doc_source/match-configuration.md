# Design a FlexMatch Matchmaker<a name="match-configuration"></a>

## Matchmaker Configurations<a name="match-configuration-elements"></a>

At a minimum, a matchmaker needs three elements:
+ The **rule set **determines the size and scope of teams for a match and defines a set of rules to use when evaluating players for a match\. Each matchmaker is configured to use one rule set\. For more information on creating rule sets, see [Build a FlexMatch Rule Set](match-rulesets.md)\.
+ The **game session queue** determines where matches created by this matchmaker will be hosted\. The matchmaker uses the queue to find available resources and to place a game session for the match\. A queue specifies which regions to use when placing a game session\. For more information on creating queues, see [Create a Queue](queues-creating.md)\.
+ The **request timeout **determines how long matchmaking requests can remain in the request pool and be evaluated for potential matches\. Once a request has timed out, it has failed to make a match and is removed from the pool\. 

In addition to these minimum requirements, you can configure your matchmaker with the following additional options\.

**Player Acceptance**  
You can configure a matchmaker to require that all players selected for a match must accept participation\. If acceptance is required, all players must be given the option to accept or reject a proposed match\. A match must receive acceptances from all players in the proposed match before it can be completed\. If any player rejects or fails to accept a match, the proposed match is discarded\. The matchmaking requests are handled as follows: requests where all players accepted the match are returned to the matchmaking pool for continued processing; requests where at least one player rejected the match or failed to respond fail and are no longer processed\. Player acceptance requires a time limit, which you can define; all players must accept a proposed match within the time limit for the match to continue\. 

**Matchmaking Notification**  
This feature emits all matchmaking\-related events to a designated Amazon Simple Notification Service \(SNS\) topic\. Since matchmaking requests are asynchronous, all games must have a way to track the status of matchmaking requests, and setting up notifications is simple and efficient\. See more on options for tracking requests in [Add FlexMatch to a Game Client](match-client.md)\. To use notifications, you first need to set up an SNS topic, and then provide the topic ARN as the notification target in the matchmaking configuration\. See more information on setting up notifications at [Set up FlexMatch Event Notification](match-notification.md)\.

**Backfill Mode**  
FlexMatch backfill is useful to keep your game sessions filled with well\-matched new players throughout the life span of the game session\. When handling backfill requests, FlexMatch uses the same matchmaker and the same process to find new players as was used to match the original players\. By setting the backfill mode in the matchmaking configuration, you can opt to use automatic backfill or manage backfill requests manually\. Learn more about automatic backfill in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

With backfill mode set to AUTOMATIC, GameLift triggers a new backfill request whenever a game session has an open player slot\. GameLift begins generating backfill requests for any game session that has open slots as soon as the game session starts up on a game server\. Automatic backfill continues unless it is explicitly turned off for a game session\. If your matchmaker uses a rule set that defines large matches \(greater than 40 players\), backfill requests that are generated automatically are given a higher priority\. This means that, as new players arrive and request a game slot, they are more likely to be placed in an existing game than in a brand new game\. 

Set backfill mode to MANUAL if you do not want to backfill your games, or if you want to manually trigger backfill requests for your games\. Manual backfill gives you the flexibility to decide when to trigger a backfill request\. For example, you may not want to add new players during certain phases of your game or only when certain conditions exist\. You can manually trigger backfill requests from either your game server \(recommended because the game server has the most up\-to\-date player counts\) or from your game client\.

**Game Properties**  
When requesting a new game session, you can pass additional information to your game server about the type of game session to create\. Game properties can be delivered with any type of game session request\-\-with a placement request or with a matchmaking request\. With matchmaking, game properties are included in the matchmaker configuration\. All game sessions created using a matchmaker use the same game properties\. If you need to vary game properties, create separate matchmakers and send matchmaking requests to the matchmaker with the appropriate game properties\.

**Reserved Player Slots**  
You can designate that certain player slots in each match be reserved and filled at a later time\. This is done by configuring the "additional player count" property of a matchmaking configuration\. 

**Custom Event Data**  
Use this property to include a set of custom information in all matchmaking\-related events for the matchmaker\. This feature can be useful for tracking certain activity unique to your game, including tracking performance of your matchmakers\. 