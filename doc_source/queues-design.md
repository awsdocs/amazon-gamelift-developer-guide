# Design a game session queue<a name="queues-design"></a>

A GameLift game session queue is a key component in your game management layer\. A queue is responsible for processing new game session requests, locating a game server to host a new game session, and prompting the game server to start the game session\. The way you design your queue determines \(1\) where GameLift can search for available game servers, and \(2\) how GameLift prioritizes available game servers to find an optimal placement for the request\. The goal of this topic is to help you design a queue that delivers the best possible experience to your players and efficiently uses the hosting resources you provision and pay for\. 

Queues are required with these GameLift features:
+ Matchmaking with FlexMatch \(see [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\)
+ Spot fleets \(see [Using Spot Instances with GameLift](spot-tasks.md)\)

For more information about queues, see [Running game sessions](gamelift-howitworks.md#gamelift-howitworks-placing)\. For information on creating a queue, see [Create a game session queue](queues-creating.md)\. For information on creating new game sessions with queues, see [Create game sessions](gamelift-sdk-client-api.md#gamelift-sdk-client-api-create)\. For help creating queues for Spot fleets, see [Tutorial: Set up a game session queue for Spot instances](tutorial-queues-spot.md)\.

## Design multiple queues as needed<a name="queues-design-players"></a>

Depending on your game and players, you may need to create more than one queue\. When your game client service requests a new game session, it specifies which game session queue to use\. Some issues to consider:
+ If you have more than one variation of your game server, create a separate queue for each variation\. All fleets in a queue must deploy compatible game servers, because players who use the queue to join games must be able to play on any of the queue's game servers\.
+ You might want to set up special queues for certain player groups\. This allows you to customize how game sessions are placed based on situation\. For example, you might need queues customized for certain game modes that require a special instance type or runtime configuration\. Or you might want a special queue to manage placements for a tournament or other event\. You can use fleets with different configurations and change how placements are prioritized\.
+ If you're not providing player latency data, you might set up queues to serve specific geographic locations\.
+ You can set up queues based on how you want game session placement metrics to be collected\. For more information, see [GameLift metrics for queues](monitoring-cloudwatch.md#gamelift-metrics-queue)\.
+ In some cases, your fleet configuration may require multiple queues\. For example, all fleets in a queue must have the same TLS certificate generation setting\.

## Build a multi\-Region queue<a name="queues-design-multiregion"></a>

A multi\-Region design is recommended for all queues\. This design can improve placement speed and hosting resiliency, and is critical when you want to use player latency data to put players into game sessions with optimal gameplay experiences\. If you're building multi\-Region queues that use Spot fleets, see [Tutorial: Set up a game session queue for Spot instances](tutorial-queues-spot.md)\.

One option is to add a [multi\-location fleet](gamelift-regions.md#gamelift-regions-hosting) to a queue\. The queue is able to place game sessions in any of the fleet's locations\. You don't need to add more fleets to increase Regional coverage, although you might want to add other fleets with different configurations or home Regions for redundancy\. If you're using multi\-location Spot fleet, follow best practices and include an On\-Demand fleet that includes the same locations\.

The following example outlines the process of designing a basic multi\-Region queue\. For this example, we'll use a game server build that is deployed in two fleets, one Spot and one On\-Demand\. Each fleet has the following locations: `us-east-1`, `us-east-2`, `ca-central-1`, and `us-west-2`\.

1. Pick a Region to create the queue in\. Our queue's location may not be significant, but if we're making placement requests through a client service, we can minimize request latency by placing the queue in a Region near where the client service is deployed\.

1. Create a new queue and add our fleets as queue destinations\. Keep in mind that the list order of destinations may affect how game sessions are placed, depending on the queue's prioritization order\. In this example, we list our Spot fleet first, and our On\-Demand fleet second\.

1. If you want to prevent game sessions from being placed in any of the destination fleet's locations, create a filter configuration that lists the locations where game sessions can be placed\. In this example, we want to place in all locations, so no filter configuration is required\.

1. Choose the queue's priority configuration to determine where the queue will search first for an available game server\. In this example, we opt to use the default priority order\. Since our game session placement requests do not include player latency data, our queue will prioritize locations based first on destinations, and then on locations\. The result of this choice is as follows:
   + The first destination listed in the queue is our Spot fleet\.
   + Fleet locations are listed alphabetically as follows: `ca-central-1`, `us-east-1`, `us-east-2`, `us-west-2`\.
   + As a result, new game sessions will always be placed in the `ca-central-1` location of the Spot fleet, unless there are no available game servers there\. In that event, the queue will then place new game sessions in `us-east-1` of the Spot fleet until it is filled, and so on\. Game sessions will only be placed in On\-Demand fleet locations if all game servers on the Spot fleet are in use\.

As our queue is put into action, we can use metric data to determine how well the queue design is holding up\.

## Prioritize where game sessions are placed<a name="queues-design-priority"></a>

GameLift uses FleetIQ algorithms to determine the best possible queue location to place a new game session\. It does this by prioritizing queue locations based on an ordered set of criteria\.

The priority criteria are: Regional player latency data \(as reported in a game session placement request\), hosting cost, geographical location \(based on a provided order, and destination fleet \(based on list order\)\. The order these criteria are applied significantly affects how placement occurs\. You can opt to use the default order or you can customize it by providing a priority configuration\.

**Default priority order**

FleetIQ applies the criteria in the following default order:
+ For placement requests that include player latency data, FleetIQ prioritizes placement in locations as follows:

  1. Lowest average latency for all players in the request\.

  1. Lowest hosting cost, if latency is equal in multiple locations\. Hosting cost is based primarily on a combination of the instance type and location \(a combination of the fleet's home Region and remote location\)\.

  1. Destination list order, if latency and cost is equal in multiple locations\. Destinations are prioritized based on the order they are listed in the queue configuration\.

  1. Location, if latency, cost, and destination are equal in multiple locations\. Locations are prioritized alphabetically based on the Region code\.
+ For placement requests that do *not* include player latency data, FleetIQ prioritizes placement in locations as follows\. This approach tends to place all game sessions on the first\-listed destination fleet, in whatever location comes first alphabetically\. Game sessions are only placed in other locations and fleets when the first location fills up\.

  1. Destination list order\. Destinations are prioritized based on the order they are listed in the queue configuration\.

  1. Location, if destination is equal in multiple locations\. Locations are prioritized alphabetically based on the Region code\.

**Custom priority order**

When customizing your queue's priority order, create a priority configuration \(see [PriorityConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_PriorityConfiguration.html)\)\. You can create a new queue with this configuration or update an existing queue\.

You can include any or all of the prioritization criteria \(`LATENCY`, `COST`, `DESTINATION`, `LOCATION`\)\. Keep in mind that FleetIQ appends any criteria that's not explicitly mentioned to the end of your list, based on the default order\. For example, if you only specify destination in your priority configuration, FleetIQ will apply the following order: \(1\) destination, \(2\) latency, \(3\) cost, \(4\) location\.

If you include LOCATION in your priority configuration, you also have to provide an ordered list of locations for FleetIQ to use\.

## Create a player latency policy<a name="queues-design-latency"></a>

If your game session placement requests include player latency data, FleetIQ works to place game sessions in locations that offer the lowest average Regional latency for all players in the request\. Making placements based on average player latency protects most players from being put into games with unacceptable latency, but it doesn't account for extreme outliers\. To accommodate these outliers, you need to put a latency cap in place\.

A latency cap policy ensures that a requested game session is not placed where any individual player in the request would experience latency over the maximum value\. Keep in mind, however, that a latency cap can also increase the risk that a game session request with latency outliers might never be fulfilled\. Instead of a single cap, you can use a series of policies to gradually raise the latency maximum over time\. You can customize how to balance providing great game experiences against getting players into games with a minimum of wait time\.

For example, you might define the following policies for a queue with a 5\-minute timeout\. Multiple policies are enforced consecutively, starting with the policy that has the lowest maximum latency value\. This set of policies starts with a maximum latency of 50 milliseconds and increases it over time to 200 milliseconds\. Your last policy sets the absolute maximum latency allowed for any player\. If you want to ensure that all game sessions are placed regardless of latency, you can set last policy's maximum to a very high value\.

1. Spend 120 seconds searching for a destination where all player latencies are less than 50 milliseconds, then\.\.\.

1. Spend 120 seconds searching for a destination where all player latencies are less than 100 milliseconds, then\.\.\.

1. Spend the remaining queue timeout searching for a destination where all player latencies are less than 200 milliseconds\.

In this example, the first policy is in force for the first 2 minutes, the second policy is in force for the third and fourth minute, and the third policy is in force for the fifth minute until the placement request times out\.

## Evaluate queue metrics<a name="queues-design-metrics"></a>

Use metrics to evaluate how well your queues are performing\. You can view queue\-specific metrics in the GameLift console \([View queue details](queues-console.md#queues-console-detail)\) or in Amazon CloudWatch\. For descriptions of queue metrics, see [GameLift metrics for queues](monitoring-cloudwatch.md#gamelift-metrics-queue)\. 

Queue metrics can provide insight in three main areas:
+ **Overall queue performance** – Metrics indicate how successfully a queue is responding to placement requests and help to identify when and why placements are failing\. For queues with manually scaled fleets, metrics average wait times and queue depth can indicate when capacity for a queue might need to be adjusted\.
+ **FleetIQ performance** – For placement requests that use FleetIQ's filtering and prioritization \(that is, requests with player latency data\), metrics indicate how often FleetIQ is able to find optimal placement for new game sessions\. Optimal placement may include finding resources with the lowest possible player latency or, when Spot fleets are available, with the lowest cost\. There are also error metrics that identify common reasons why optimal placement failed\.
+ **Region\-specific placements** – For multi\-Region queues, metrics show successful placements by broken down by Region\. With queues that use FleetIQ, this data provides useful insight into where player activity is occurring\.

When evaluating metrics for FleetIQ performance, consider the following tips:
+ Use the "placements succeeded" metric in combination with the FleetIQ metrics for lowest latency and/or lowest price to track the queue's rate of optimal placement\.
+ To boost a queue's optimal placement rate, take a look at the error metrics for the following:
  + If the error metric "first choice not available" is high, this is a good indicator that capacity scaling for the queue's fleets needs to be adjusted\. It may be that all fleets in the queue are under\-scaled, or there may be one particular fleet or Region that is an optimal fit for most placements\.
  + If the error metric "first choice not viable" is high, this is an indicator to look at your Spot fleets\. Spot fleets are considered "not viable" when the interruption rate for a particular instance type is too high\. To resolve this issue, change the queue to use Spot fleets with different instance types\. As noted in the best practices for queues with Spot fleets, it is always a good idea to include Spot fleets with different instance types in each Region\.