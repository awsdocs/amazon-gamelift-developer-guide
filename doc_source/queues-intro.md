# Setting up GameLift queues for game session placement<a name="queues-intro"></a>

A game session queue is the primary mechanism for processing new game session requests and locating available game servers to host them\. Queues offer significant benefits for game developers and players\. These include:
+ **Queues provide best possible placement\.** When processing game session placement requests, a queue uses GameLift algorithms to prioritize queue locations based on a set of defined preferences\.
+ **Host games on lower\-priced Spot fleets\.** Use queues to optimize use of AWS Spot fleets, which offer significantly lower hosting costs\. By default, queues always try to place new game sessions in Spot fleets\. 
+ **Place new games faster during high demand\.** Queues use multiple possible locations for placements\. This means that there is always fallback capacity if the preferred placement location is unavailable\.
+ **Make game availability more resilient\.** Outages can happen\. With a multi\-Region queue, a slowdown or outage doesn't have to affect player access to your game\.
+ **Use extra fleet capacity more efficiently\.** To handle unexpected surges in player demand, queues provide quick access to extra hosting capacity\. The fleet locations in a queue provide back\-up capacity for each other\. Locations scale up or down based on player demand\.
+ **Get metrics on game session placements and queue performance\.** GameLift emits queue metrics, including statistics on placement successes and failures, the number of requests in the queue, and average time that requests spend in the queue\. You can view these metrics in the GameLift console or in CloudWatch\.

## How GameLift queues works<a name="queues-design-fleetiq"></a>

GameLift uses an algorithm that manages how a game session queue selects hosting resources for game session placements\. You configure a queue with fleet destinations where GameLift can place game sessions\. GameLift relies on the following decision making process when searching for the best possible placement for a new game session\.

1. Filters the queue's destinations to remove any of the following fleets:
   + Fleets in Regions where the reported latency exceeds a policy's maximum limit\. 
   + Spot fleets that aren't usable due to unacceptable interruption rates\.

1. Prioritizes the remaining queue destinations based on the following:
   + Re\-orders the queue destinations by lowest average player latency\.
   + If player latency data is **not** provided, GameLift uses the original list of queue destinations\.

1. Selects a destination from the prioritized list\. 
   + If the destination list was prioritized by Region, GameLift selects the fleet in the lowest\-latency Region with the lowest price\.
   + If the destination list was **not** prioritized, GameLift selects the first usable fleet from the original list\.

1. Evaluates if the selected fleet has a server process available to host a new game session\.

1. If the selected fleet has no available resources, GameLift moves to the next listed destination and repeats until it finds a fleet to host the new game session\.

    