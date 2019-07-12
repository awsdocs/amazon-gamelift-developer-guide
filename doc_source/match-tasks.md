# FlexMatch Integration Roadmap<a name="match-tasks"></a>

To add FlexMatch matchmaking to your game, complete the following tasks\. 
+ **Set up a matchmaker\.** A matchmaker receives matchmaking requests from players and processes them\. It groups players based on a set of defined rules and, for each successful match, creates a new game sessions and player sessions\. Follow these steps to set up a matchmaker:
  + **Create a rule set\.** A rule set tells the matchmaker how to construct a valid match\. It specifies team structure and specifies how to evaluate players for inclusion in a match\. See these topics: 
    + [Build a FlexMatch Rule Set](match-rulesets.md)
    + [FlexMatch Rule Set Examples](match-examples.md)
  + **Create a game session queue\.** A queue locates the best region for each match and creates a new game session in that region\. Use an existing queue or create a new one for matchmaking\. See this topic: 
    + [Create a Queue](queues-creating.md)
  + **Set up notifications \(optional\)\.** Since matchmaking requests are asynchronous, you need a way to track the status of requests\. Notifications is the preferred option\. See this topic: 
    + [Set up FlexMatch Event Notification](match-notification.md)
  + **Configure a matchmaker\.** Once you have a rule set, queue, and notifications target, create the configuration for your matchmaker\. See these topics:
    + [Design a FlexMatch Matchmaker](match-configuration.md)
    + [Create a Matchmaking Configuration](match-create-configuration.md)
+ **Integrate FlexMatch into your game client service\.** Add functionality to your game client service to start new game sessions with matchmaking\. Requests for matchmaking specify which matchmaker to use and provide the necessary player data for the match\. See this topic: 
  + [Add FlexMatch to a Game Client](match-client.md)
+ **Integrate FlexMatch into your game server\.** Add functionality to your game server to start game sessions that are created through matchmaking\. Requests for this type of game session include match\-specific information, including players and team assignments\. The game server needs to access and use this information when constructing a game session for the match\. See this topic:
  + [Add FlexMatch to a Game Server](match-server.md)
+ **Set up FlexMatch backfill \(optional\)\.** Request additional player matches to fill open player slots in existing games\. You can turn on automatic backfill to have GameLift manage backfill requests\. Or you can manage backfill manually by adding functionality to your game client service or game server to initiate match backfill requests\. See this topic:
  + [Backfill Existing Games with FlexMatch](match-backfill.md)
**Note**  
FlexMatch backfill is currently not available for games using Realtime Servers\.