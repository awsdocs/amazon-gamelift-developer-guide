# Design a Game Session Queue<a name="queues-design"></a>

Amazon GameLift uses queues to process requests for game session placements and find hosting resources for new game sessions\. The way you design your queue determines \(1\) where Amazon GameLift looks for available resources, and \(2\) how Amazon GameLift evaluates available resources to find the optimal choice for each new game session\. Use queues to deliver the best possible experience to your players and to ensure that the hosting resources you're paying for are utilized efficiently\. 

Queues are required with these Amazon GameLift features: 
+ Matchmaking with FlexMatch \(see [Design a FlexMatch Matchmaker](match-tasks.md)\)
+ Spot fleets \(see [Spot Fleet Integration Guide](spot-tasks.md)\)

Learn more about queues in [Running Game Sessions](gamelift-howitworks.md#gamelift-howitworks-placing)\. For information on creating a queue, see [Create a Queue](queues-creating.md)\. For information on creating new game sessions with queues, see [Create Game Sessions](gamelift-sdk-client-api.md#gamelift-sdk-client-api-create)\. 

## Why Use Queues?<a name="queues-design-benefits"></a>

We highly recommend using queues for game session placement even if you're not using an Amazon GameLift feature that requires them\. While you always have the option to manually create a game session on a specific fleet \([CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html)\), queues—particularly those that span multiple regions—can offer critical benefits for you and your players\.
+ **Minimize latency for a better player experience\.** When a game session placement request includes player latency data, FleetIQ ensures that players get the lowest possible latency to the game server\. Set additional rules to minimize latency differences between players and to strike a balance between getting players into games fast and getting players into the best possible gaming experience\. 
+ **Take advantage of lower\-priced spot fleets\.** You can add spot fleets, which offer significantly lower hosting costs, to a queue at any time\. When spot fleets are available, FleetIQ places new game sessions in fleets with the lowest possible latency **and** the lowest spot price\.
+ **Place new games faster at high capacity\.** A single fleet has limited capacity, and once that limit is reached, players must wait until additional instances are scaled up before new game sessions can start\. A queue, however, can fall back to immediately make placements on other fleets if the preferred fleet is full\. In addition, with auto\-scaling, each fleet in the queue scales up as it nears full capacity, so it's unlikely that all fleets in a queue will be full at the same time\. As a result, wait time for players is impacted less during surges in player demand\.
+ **Make game availability more resilient\.** Fleet and region\-level outages can happen\. When using a multi\-region queue, a slowdown or outage doesn't have to affect player access to your game\. Instead, if one or more preferred fleets are unavailable, Amazon GameLift can place new game sessions with the next best fleet\. Auto\-scaling then adjusts for this temporary shift in fleet activity until the preferred fleet\(s\) are available again\.
+ **Use extra fleet capacity more efficiently\.** To handle unexpected surges in player demand, it makes sense to have quick access to extra hosting capacity\. When relying on a single fleet to support player access to your game, you need to maintain unused capacity just in case\. In contrast, the fleets in a queue can act as fall backs, with increased player demand shifting to lesser used fleets in other regions as needed\. For example, when demand is high in Asia, demand is typically low in Europe; your European fleets can provide additional capacity to support surges in Asia, even when they're scaled down during low demand\.
+ **Get metrics on game session placements and queue performance\.** Amazon GameLift emits queue\-specific metrics, including statistics on placement successes and failures, the number of requests in the queue, and average time that requests spend in the queue\. You can view these metrics in the Amazon GameLift console or in CloudWatch\.

## Tips on Setting Queue Destinations<a name="queues-design-destinations"></a>

A queue contains a list of destinations where game session placement requests can be fulfilled\. In most cases a destination is a fleet, which you can specify by either a fleet ID or alias ID\. When choosing destinations for the queue, consider the following guidelines and best practices:
+ You can add any existing fleet or alias from any region\. Add a fleet in each region where you want to support players, especially if you're using player latency for placement\.
+ If you assign aliases to your fleets \(as recommended\), it is a good idea to use the alias names when setting destinations in your queue\.
+ All destinations in a queue must be running game builds that are compatible with the game clients that use the queue\. Keep in mind that new game session requests processed by the queue might be placed on any destination in the queue\. 
+ A queue should have at least two fleets and span at least two regions\. This design improves hosting resiliency by decreasing the impact of fleet or regional slowdowns and more efficiently managing unexpected changes in player demand\.
+ The list order of destinations in a queue matters\. If you include latency data in a game session placement request, Amazon GameLift re\-prioritizes the destinations to find available resources with \(1\) minimal player latency and \(2\) lowest spot price \(if relevant\)\. If you don't provide latency data, Amazon GameLift follows the destination list order\. In this situation, game sessions will usually be hosted on the first fleet listed and will only be placed on back\-up fleets when needed\. 
+ You need to decide where to create your queue \(in which region\)\. Ideally you're making game session placement requests through a game client service, such as a session directory service\. If so, we recommend that you create your queue in a region that is geographically near where the client service is deployed\. This arrangement minimizes latency when submitting game session placement requests\. 
+ A queue must not have fleets with different certificate configurations\. All fleets in the queue must have TLS certificate generation either enabled or disabled\.

## Design a Multi\-Region Queue<a name="queues-design-multiregion"></a>

A multi\-region design is recommended for all queues, whether or not you're using special features such as FlexMatch matchmaking or spot fleets\. This design can improve placement speed and hosting resiliency, and is critical when working with player latency data\. 

This topic steps through the process of designing a basic multi\-region queue\. For this example, we'll use a game server build that we want to distribute to players on the east coast of North America\. We've chosen to host the our game in the following regions: `us-east-1` and `ca-central-1`\. We've uploaded the game build and created a fleet in each region\.

1. Pick a region to create the queue in\. Our queue's location may not be significant, but if we're making placement requests through a client service, we can minimize request latency by placing the queue in a region near where the client service is deployed\.

1. Create a new queue and add our fleets as queue destinations\. 

1. Decide on a default list order for the destinations\. GameLift may evaluate fleets in this order when searching for available hosting resources\. If a placement request contains player latency data, Amazon GameLift reorders our destinations to prioritize the lowest latency rates\. If no latency data is provided, Amazon GameLift uses the default order\. 
   + If we expect a lot of requests without latency data, we'll want to pay more attention to our default list order and the fleets we list first\. Without latency data, Amazon GameLift will always place new game sessions on the first fleet listed when available, only using the remaining fleets as backup\. As a result, we'll want a fleet in the top spot that \(1\) is high\-powered enough to handle most of our player base, and \(2\) is likely to deliver the highest availability and lowest latency to the majority of our players\. 

     In this scenario, we expect that the `us-east-1` fleet will best serve most of our players, so we choose a high\-performance instance type, configure it for multiple concurrent game sessions, scale for high player volume, and list it first in our queue\. Our backup `ca-central-1` fleet uses a smaller, less expensive instance type and maintains minimal capacity\. Additional backup fleets \(if added\) do not need to be ordered, but we might de\-prioritize regions that are likely to have high latency rates\. 
   + If we expect most requests to have latency data, the list order is less important because FleetIQ will re\-prioritize it\. We'll want to have fleets in all regions that are close to our target players to minimize latency rate\. Generally, we'll want fleets that are configured to handle the expected amount of player demand\. We'll want to make sure that the fleet in the top spot is a good choice to handle the requests that don't have latency data\.

     In this scenario, we expect that both the `us-east-1` and `ca-central-1` fleets will provide low latency rates and be heavily used\. Both fleets use high\-performance instance types and are configured for multiple concurrent game sessions\. Demand for the `ca-central-1` fleet will be somewhat lower, so it will maintain a lower capacity\.

As our queue is put into action, we can use metric data to determine how well the design is holding up\. With queues, we make fleet changes as needed, either by reconfiguring existing fleets or by removing and adding new fleets that better fit our hosting needs\.

## Design Player Latency Policies<a name="queues-design-latency"></a>

If your game session placement requests include player latency data, Amazon GameLift uses FleetIQ to ensure that players are put into game sessions with the lowest possible latency\. FleetIQ prioritizes a queue's destinations based on the average regional latency for all players in the request\. 

You can set up player latency policies to affect how Amazon GameLift uses player latency data\. You can set player latency policies to: 
+ Set maximum latency for individual players\. Amazon GameLift by default places game sessions based on average latency across all players in the request\. This policy ensures that a game session is not placed where any one player will experience latency over the maximum\.
+ Use multiple policies to relax maximum latency over time\. Setting a maximum latency protects players from high\-latency games, but they also increase the risk that a game session request with high latency values will never be fulfilled\. You can use a series of policies to gradually raise the latency maximum over time\. Use this approach to balance how you provide great game experiences against getting players into games with a minimum of wait time\. 

For example, you might define the following policies for a queue with a 5\-minute timeout\. Multiple policies are enforced consecutively, starting with the policy that has the lowest maximum latency value\. This set of policies starts with a maximum latency of 50 milliseconds and increases it over time to 200 milliseconds\. Your last policy sets the absolute maximum latency allowed for any player\. If you want to ensure that all game sessions are placed regardless of latency, you can set last policy's maximum to a very high value\.

1. Spend 120 seconds searching for a destination where all player latencies are less than 50 milliseconds, then\.\.\.

1. Spend 120 seconds searching for a destination where all player latencies are less than 100 milliseconds, then\.\.\.

1. Spend the remaining queue timeout searching for a destination where all player latencies are less than 200 milliseconds\.

In this example, the first policy is in force for the first two minutes, the second policy is in force for the third and fourth minute, and the third policy is in force for the fifth minute until the placement request times out\. 

## Design a Queue for Spot Instances<a name="queues-design-spot"></a>

If you plan to use spot instances for your fleets, you'll need to set up a queue\. Ideally, you want to set up a resilient queue that takes advantage of cost savings with spot fleets while minimizing the chance of game session interruptions\. Use the following best practices:
+ Include fleets from more than one region\. A multi\-region queue boosts resiliency by ensuring that there are always fleets available to host new game sessions\.
+ In each region, include at least one spot fleet and one on\-demand fleet\. This design ensures that game sessions can be placed in any region even if no spot fleets are currently viable in that region\.
+ If you include multiple spot fleets in each region, use different instance types for each fleet, preferably in the same instance family \(C4\.large, C4\.xlarge, etc\.\)\. This design lessens the likelihood that multiple spot fleets will be unavailable or interrupted at the same time\. Check the historical pricing data in the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/) to verify that your preferred instance types typically deliver significant cost savings with spot\. This data displays pricing for spot and on\-demand instances and provides estimated spot saving per instance\.
+ To optimize FleetIQ's ability to select the lowest priced spot instance, plan to include player latency data for all regions\. As described in [How FleetIQ Works](#queues-design-fleetiq), FleetIQ works more effectively when destinations are re\-prioritized by region, which is done when latency data is provided\.
+ If you're not providing player latency data in game session placement requests, order your destinations by preference\. For example, you might list destinations by region preference \(spot fleet followed by on\-demand fleet\)\. Alternatively, you might list all your spot fleets first\.

## How FleetIQ Works<a name="queues-design-fleetiq"></a>

FleetIQ relies on the following decisionmaking process when searching for the best possible placement for a new game session\. 

1. FleetIQ filters the queue's destinations to remove any of the following fleets:
   + If the request includes player latency data, FleetIQ evaluates the queue's player latency policies and removes all fleets in regions where any player's latency exceeds a policy's maximum limit\. 
   + FleetIQ removes any spot fleets that are not currently viable due to unacceptable interruption rates\.

1. FleetIQ prioritizes the remaining queue destinations based on the following:
   + If player latency data is provided, FleetIQ re\-orders the queue destinations by region, those with lowest average player latency listed first\.
   + If player latency data is **not** provided, FleetIQ uses the original list of queue destinations\.

1. FleetIQ selects a destination from the prioritized list\. 
   + If the destination list was prioritized by region, FleetIQ selects the fleet in the lowest\-latency region with the lowest price\. If there are no viable spot fleets, any fleet in that region may be selected\.
   + If the destination list was **not** prioritized, FleetIQ selects the first viable fleet from the original list, even if there are lower priced spot fleets on the list\.

1. FleetIQ evaluates whether the selected fleet has a server process available to host a new game session\. It is considered an "optimal" placement when the new game session is placed in a fleet with the lowest possible latency and/or the lowest price\. 

1. If the selected fleet has no available resources, FleetIQ moves to the next listed destination and repeats until it finds a fleet to host the new game session\.

## Evaluate Queue Metrics<a name="queues-design-metrics"></a>

Use metrics to evaluate how well your queues are performing\. You can view queue\-specific metrics in the Amazon GameLift console \([View Queue Details](queues-console.md#queues-console-detail)\) or in Amazon CloudWatch\. Queue metrics are described in [Amazon GameLift Metrics for Queues](monitoring-cloudwatch.md#gamelift-metrics-queue)\. 

Queue metrics can provide insight in three main areas:
+ **Overall queue performance** – Metrics indicate how successfully a queue is responding to placement requests and help to identify when and why placements are failing\. For queues with manually scaled fleets, metrics average wait times and queue depth can indicate when capacity for a queue might need to be adjusted\. 
+ **FleetIQ performance** – For placement requests that use FleetIQ's filtering and prioritization \(that is, requests with player latency data\), metrics indicate how often FleetIQ is able to find optimal placement for new game sessions\. Optimal placement may include finding resources with the lowest possible player latency or, when spot fleets are available, with the lowest cost\. There are also error metrics that identify common reasons why optimal placement failed\.
+ **Region\-specific placements** – For multi\-region queues, metrics show successful placements by broken down by region\. With queues that use FleetIQ, this data provides useful insight into where player activity is occurring\.

When evaluating metrics for FleetIQ performance, consider the following tips:
+ Use the "placements succeeded" metric in combination with the FleetIQ metrics for lowest latency and/or lowest price to track the queue's rate of optimal placement\.
+ To boost a queue's optimal placement rate, take a look at the error metrics for the following: 
  + If the error metric "first choice not available" is high, this is a good indicator that capacity scaling for the queue's fleets needs to be adjusted\. It may be that all fleets in the queue are under\-scaled, or there may be one particular fleet or region that is an optimal fit for most placements\. 
  + If the error metric "first choice not viable" is high, this is an indicator to look at your spot fleets\. Spot fleets are considered "not viable" when the interruption rate for a particular instance type is too high\. To resolve this issue, change the queue to use spot fleets with different instance types\. As noted in the best practices for queues with spot fleets, it is always a good idea to include spot fleets with different instance types in each region\.