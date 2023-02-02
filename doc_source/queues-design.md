# Design a game session queue<a name="queues-design"></a>

A GameLift game session queue is a key component in your game management layer\. Your queue design determines where GameLift searches for available game servers and how GameLift prioritizes available servers to find an ideal placement for the game session\. This topic shows how to design a queue that delivers a player experience with minimal latency and that efficiently uses hosting resources\. For more information about game session queues and how they work, see [Setting up GameLift queues for game session placement](queues-intro.md)\.

These GameLift features require queues:
+ [Matchmaking with FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)
+ [Use Spot Instances with GameLift](spot-tasks.md)

## Create a player latency policy<a name="queues-design-latency"></a>

If your game session placement requests include player latency data, the FleetIQ algorithm finds game sessions in locations that offer the lowest average latency for all players in the request\. Placing game sessions based on average player latency prevents GameLift from placing most players in games with high latency\. However, GameLift still places players with extreme latency \. To accommodate these players, create player latency policies\.

A player latency policy prevents GameLift from placing a requested game session anywhere that players in the request would experience latency over the maximum value\. Player latency policies can also prevent GameLift from matching game session requests with higher latency players\.

For example, consider this queue with a 5\-minute timeout and the following player latency policies:

1. Spend 120 seconds searching for a location where all player latencies are less than 50 milliseconds\.

1. Spend 120 seconds searching for a location where all player latencies are less than 100 milliseconds\.

1. Spend the remaining queue time until timeout searching for a location where all player latencies are less than 200 milliseconds\.

![\[The Create queue page in the GameLift console, showing a queue that has the Timeout set to 600 seconds and that includes three player latency policies. The first policy starts at 0 seconds and end at 120 seconds with a maximum player latency of 50 milliseconds. The second policy starts at 120 seconds and end at 240 seconds with a maximum player latency of 100 milliseconds. The final policy starts at 240 seconds and ends at 600 seconds with a maximum player latency of 200 milliseconds.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/queue-latency-policy.png)

## Build a multi\-location queue<a name="queues-design-multiregion"></a>

We recommend a multi\-location design for all queues\. This design can improve placement speed and hosting resiliency\. A multi\-location design is required to use player latency data to put players into game sessions with minimal latency\. If you're building multi\-location queues that use Spot Instance fleets, follow the instructions in [Tutorial: Set up a game session queue for Spot instances](tutorial-queues-spot.md)\.

One way to create a multi\-location queue is to add a [multi\-location fleet](gamelift-regions.md#gamelift-regions-hosting) to a queue\. That way, the queue can place game sessions in any of the fleet's locations\. You can also add other fleets with different configurations or home locations for redundancy\. If you're using a multi\-location Spot Instance fleet, follow best practices and include an On\-Demand Instance fleet with the same locations\.

The following example outlines the process of designing a basic multi\-location queue\. In this example, we use two fleets: one Spot Instance fleet and one On\-Demand Instance fleet\. Each fleet has the following AWS Regions for placement locations: `us-east-1`, `us-east-2`, `ca-central-1`, and `us-west-2`\.

**To create a basic multi\-location queue with multi\-location fleets**

1. Choose a location to create the queue in\. You can minimize request latency by placing the queue in a location near where you deployed the client service\. In this example, we create the queue in `us-east-1`\.

1. Create a new queue and add your multi\-location fleets as queue destinations\. The destination order determines how GameLift places game sessions\. In this example, we list the Spot Instance fleet first and the On\-Demand Instance fleet second\.

1. Define the queue's game session placement priority order\. This order determines where the queue searches first for an available game server\. In this example, we use the default priority order\.

1. Define the location order\. If you don't define the location order, GameLift uses the locations in alphabetical order\.

![\[The Create queue page of the GameLift console, displaying the Game session placement locations, Destination order, Game sessions placement priority, and Location order sections. In the Game session placement locations section, the ca-central-1, us-west-2, us-east-2, and us-east-1 AWS Regions are all selected. In the Destination order section, a fleet named TestFleet-SPOT in us-east-1 is first, and a fleet named TestFleet-ONDEMAND in us-east-1 is second.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/queue-multi-location-1.png)

![\[In the Game session placement priority section, the default priority order is defined: latency, cost, destination, location. In the Location order section, the location order is defined as: ca-central-1, us-east-1, us-east-2, us-west-2.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/queue-multi-location-2.png)

## Prioritize where game sessions are placed<a name="queues-design-priority"></a>

GameLift uses the FleetIQ algorithm to determine where to place a new game session based on an ordered set of criteria\. You can use the default priority order, or you can customize the order\.

**Default priority order**

For placement requests that include player latency data, FleetIQ prioritizes the game session placement criteria in the following default order:

1. **Latency** – Lowest average latency for all players in the request\.

1. **Cost** – Lowest hosting cost, if latency is equal in multiple locations\. Hosting cost is primarily based on a combination of the instance type and location\.

1. **Destination** – Destination order, if latency and cost are equal in multiple locations\. FleetIQ prioritizes destinations based on the order listed in the queue configuration\.

1. **Location** – Location order, if latency, cost, and destination are equal in multiple locations\. FleetIQ prioritizes locations based on the order listed in the queue configuration\.

**Custom priority order**

To customize a queue's priority order in the [GameLift console](https://console.aws.amazon.com/gamelift), drag the priority value to the position that you want it in\. To customize a queue's priority order using the AWS Command Line Interface \(AWS CLI\), use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session-queue.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session-queue.html) command with the \-\-priority\-configuration option\. You can use this command to create a new queue or to update an existing queue\.

The FleetIQ algorithm appends any criteria not explicitly mentioned to the end of your list, based on the default order\. If you include the location criterion in your priority configuration, you must also provide an ordered list of locations\.

## Design multiple queues as needed<a name="queues-design-players"></a>

Depending on your game and players, you might want to create more than one game session queue\. When your game client service requests a new game session, it specifies which game session queue to use\. To help you determine whether to use multiple queues, consider:
+ Variations of your game server\. You can create a separate queue for each variation of your game server\. All fleets in a queue must deploy compatible game servers\. This is because players who use the queue to join games must be able to play on any of the queue's game servers\.
+ Different player groups\. You can customize how GameLift places game sessions based on player group\. For example, you might need queues customized for certain game modes that require a special instance type or runtime configuration\. Or, you might want a special queue to manage placements for a tournament or other event\.
+ Game session queue metrics\. You can set up queues based on how you want to collect game session placement metrics\. For more information, see [GameLift metrics for queues](monitoring-cloudwatch.md#gamelift-metrics-queue)\.

## Evaluate queue metrics<a name="queues-design-metrics"></a>

Use metrics to evaluate how well your queues are performing\. You can view metrics related to queues in the [GameLift console](https://console.aws.amazon.com/gamelift) or in Amazon CloudWatch\. For a list and descriptions of queue metrics, see [GameLift metrics for queues](monitoring-cloudwatch.md#gamelift-metrics-queue)\.

Queue metrics can provide insight about the following:
+ **Overall queue performance** – Queue metrics indicate how successfully a queue responds to placement requests\. These metrics can also help you identify when and why placements fail\. For queues with manually scaled fleets, the `AverageWaitTime` and `QueueDepth` metrics can indicate when you should adjust capacity for a queue\.
+ **FleetIQ algorithm performance** – For placement requests that use the FleetIQ algorithm for filtering and prioritization, queue metrics indicate how often the FleetIQ algorithm can find an ideal placement for new game sessions\. The ideal placement may prioritize using resources with the lowest possible player latency or, when Spot Instance fleets are available, resources with the lowest cost\. There are also error metrics that identify common reasons why GameLift can't find an ideal placement\. For more information on metrics, see [Monitor GameLift with Amazon CloudWatch](monitoring-cloudwatch.md)\.
+ **Location specific placements** – For multi\-location queues, metrics show successful placements by location\. For queues that use the FleetIQ algorithm, this data provides useful insight into where player activity occurs\.

When evaluating metrics for FleetIQ algorithm performance, consider the following tips:
+ To track the queue's rate of finding an ideal placement, use the `PlacementsSucceeded` metric in combination with the FleetIQ metrics for lowest latency and lowest price\.
+ To boost a queue's rate of finding an ideal placement, review the following error metrics:
  + If the `FirstChoiceOutOfCapacity` is high, adjust capacity scaling for the queue's fleets\.
  + If the `FirstChoiceNotViable` error metric is high, look at your Spot Instance fleets\. Spot Instance fleets are considered not viable when the interruption rate for a particular instance type is too high\. To resolve this issue, change the queue to use Spot Instance fleets with different instance types\. It's always a good idea to include Spot Instance fleets with different instance types in each location\.