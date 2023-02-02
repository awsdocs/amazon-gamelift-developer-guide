# Best practices for GameLift game session queues<a name="queues-best-practices"></a>

Here are some best practices that can help you build effective game session queues for game session placement\.

## Best practices for queues with any fleet type<a name="queues-design-destinations"></a>

A queue contains a list of fleet destinations where new game sessions can be placed\. Each fleet can have instances deployed in multiple geographic locations\. When choosing a placement, the queue selects a combination of a fleet and a fleet location\. You provide a set of priorities for the queue to use when choosing a placement\.

Consider the following guidelines and best practices:
+ You can add fleets and aliases that reside in any AWS Region\. Whether you're using fleets with a single location or multiple locations, make sure you add fleets that cover every geographical location where you want to support players\. This is particularly important if you're making placements based on reported player latency\.
+ Assign an alias to each fleet in a queue, and use the alias names when setting destinations in your queue\.
+ Destinations in a queue must be running game builds that are compatible with the game clients that use the queue to get new game sessions\. New game session requests that are processed by the queue can potentially be placed on any queue destination\.
+ A queue should have at least two fleets that reside in different Regions\. This design improves hosting resiliency by decreasing the impact of fleet or Regional slowdowns\. It also enables the queue to more efficiently manage unexpected changes in player demand\.
+ The list order of destinations in a queue matters\. A queue prioritizes placement choices based on several elements, including destination list order\. If a queue is configured to prioritize on destination, game sessions will always be placed on the first fleet listed, if there's an available game server to host it\.
+ When deciding what Region to create your game session queue in, consider where most of its traffic will be coming from\. In most games, a game client service, such as a session directory service, manages new game session requests\. By positioning your queue in a Region that is geographically close to where your client service is deployed, you can minimize communication latency\.
+ When choosing fleets to provide hosting in specific locations, you can use [multi\-location fleets](gamelift-regions.md#gamelift-regions-hosting) that cover a larger set of locations than you want\. Use the queue filter configuration to prevent the queue from placing game sessions in specified locations\. You can use at least two multi\-location fleets with different home Regions to mitigate the impact of game placements during a Regional outage\.
+ A queue cannot have fleets with different certificate configurations\. All fleets in the queue must have TLS certificate generation either enabled or disabled\.

## Best practices for queues with Spot fleets<a name="queues-design-spot"></a>

If your queue includes Spot fleets, you want to set up a resilient queue that takes advantage of cost savings with Spot fleets while minimizing the effect of game session interruptions\. For help correctly building fleets and game session queues for use with Spot fleets, see [Tutorial: Set up a game session queue for Spot instances](tutorial-queues-spot.md)\. For more information about Spot instances, see [Use Spot Instances with GameLift](spot-tasks.md)\.

In addition to the general best practices in the previous section, consider these Spot\-specific best practices:
+ Include at least one Spot fleet and one On\-Demand fleet to cover all the geographic locations where you want to support players\. This design is critical to minimizing the impact of potential Spot interruptions\. It ensures that game sessions can be placed in any location, even when no Spot fleets are currently viable there\. At a minimum, every queue should have at least one On\-Demand fleet\.
+ Include fleets that use different instance types, preferably in the same instance family \(c5\.large, c5\.xlarge, etc\.\)\. This design lessens the likelihood that a Spot interruption, which usually affects a specific instance type, will impact all instances in the same location\. Check the historical pricing data in the [GameLift console](https://console.aws.amazon.com/gamelift/) to verify that your preferred instance types typically deliver significant cost savings with Spot\. You can view pricing for Spot and On\-Demand instances as well as estimated Spot savings per instance\.
+ When configuring priorities for a queue with Spot fleets, place cost near the top of the list\. This will ensure that locations on Spot fleets will always take precedence over locations on On\-Demand fleets, when available\.