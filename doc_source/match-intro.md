# Adding FlexMatch Matchmaking<a name="match-intro"></a>

Use Amazon GameLift FlexMatch to add player matchmaking functionality to your games\. FlexMatch pairs the matchmaking service with a customizable rules engine\. This lets you design how to match players together based on player attributes and game modes that make sense for your game, and rely on FlexMatch to manage the nuts and bolts of forming player groups and placing them into games\. 

FlexMatch builds on the Queues feature\. Once a match is formed, FlexMatch hands the match details to a queue of your choice\. The queue searches for available hosting resources on your Amazon GameLift fleets and starts a new game session for the match\.

The topics in this section cover how to add matchmaking support to your game servers and game clients\. To create a matchmaker for your game, see [Setting Up Amazon GameLift FlexMatch Matchmakers](matchmaker-build.md)\. For more information on how FlexMatch works, see [How Amazon GameLift FlexMatch Works](gamelift-match.md)\.

**Topics**
+ [FlexMatch Integration Roadmap](match-tasks.md)
+ [Add FlexMatch to a Game Client](match-client.md)
+ [Add FlexMatch to a Game Server](match-server.md)
+ [Backfill Existing Games with FlexMatch](match-backfill.md)