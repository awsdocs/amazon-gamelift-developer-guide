# Adding FlexMatch matchmaking<a name="gamelift-match-intro"></a>

Use GameLift FlexMatch to add player matchmaking functionality to your GameLift hosted games\. You can use FlexMatch with either custom game servers or Realtime Servers\. 

FlexMatch pairs the matchmaking service with a customizable rules engine\. You design how to match players together based on player attributes and game modes that make sense for your game\. FlexMatch manages the nuts and bolts of evaluating players who are looking for a game, forming matches with one or more teams, and starting game sessions to host the matches\. 

To use the full FlexMatch service, you must have your hosting resources set up with queues\. GameLift uses queues to locate the best possible hosting locations for games across multiple regions and computing types\. In particular, GameLift queues can use latency data, when provided by game clients, to place game sessions so that players experience the lowest possible latency when playing\.

For more information on FlexMatch including detailed help with integrating matchmaking into your games, see these [GameLift FlexMatch Developer Guide](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/) topics:
+ [How GameLift FlexMatch works](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html)
+ [FlexMatch integration steps](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)