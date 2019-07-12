# Create Game Sessions with Queues<a name="gamelift-sdk-client-queues"></a>

This set of features lets you place new game sessions more efficiently across Amazon GameLift resources, and better supports matchmaking services\. Previously, new game session requests were limited to a single fleet \([CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html)\), and the request failed if the fleet was at full capacity or otherwise compromised\. 

Use a queue to place new game sessions on any one of a group of fleets that can span regions\. Increased player demand can be shifted to lesser used fleets in other regions as needed\. Queues also decrease the overhead needed to monitor fleets and balance resources across multiple fleets and regions\. You can manage queues and track queue performance metrics in the Amazon GameLift Console\. 

Create a new game session placement request and add it to a queue\. A game session placement request includes standard game session properties, and also lets you add one or more players to the new game session\. 

When creating a game session placement request, include player latency data to help Amazon GameLift choose a fleet in a region that provides the best possible experience for all the players\. 