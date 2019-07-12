# Using Multi\-Region Queues<a name="queues-intro"></a>

Use Queues to create fleet groups that span multiple regions, and allow game session placement in any fleet in the queue\. Queues offer numerous advantages over using individual fleets, including improved efficiency of game session placements and the ability to use player latency when selecting a game location\. Client servers service specify a queue when requesting new game session placements or matchmaking\. Use the AWS Command Line Interface \(CLI\) or the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/) to create, edit, and track queue metrics\.

**Topics**
+ [Design a Game Session Queue](queues-design.md)
+ [Create a Queue](queues-creating.md)
+ [View Your Queues](queues-console.md)