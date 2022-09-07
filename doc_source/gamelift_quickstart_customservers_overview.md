# Hosting components overview<a name="gamelift_quickstart_customservers_overview"></a>

This section provides a high\-level overview of the different components in a custom server\. It also provides an example game session placement flow\.

**GameLift FlexMatch matchmaking configuration and rule set** – If you want to use the matchmaking capabilities of GameLift, you must:
+ Define a matchmaking configuration\.
+ Provide a JSON document\-based rule set\.
+ Use the GameLift matchmaking API operations from your backend service\.

**GameLift fleets** – A fleet contains Amazon Elastic Compute Cloud \(Amazon EC2\) instances that GameLift hosts\. A fleet uses the configuration and scaling logic that you define to run your game server build\. You can use a fleet directly without a queue\. You can also associate multiple fleets with a GameLift queue\. For example, you can use Spot Instance fleets configured with your preferred locations, along with a backup On\-Demand Instance fleet with the same locations\. Using multiple Spot Instance fleets of different [instance types](gamelift-ec2-instances.md#gamelift-ec2-instances-type) reduces the chance of needing On\-Demand Instance placement\.

**GameLift queue** – To place game sessions globally across GameLift fleets, you must create a GameLift queue\. The queue configuration contains a priority order for your fleets, and also your latency requirements\. GameLift uses the queue to place game sessions based on the lowest cost and latency\.

With multi\-Region fleets, you don't need to separate fleets per AWS Region\. The queue can manage placement across multiple locations within a single fleet\. When the fleets don't need custom configuration or builds, you don't need to run different fleets in all the different Regions\. However, because you manage fleets in your [home Region](gamelift-regions.md#gamelift-regions-hosting), we recommend that you have fleets in two different home Regions for redundancy\.

**Game backend service** – You must always host a backend service for your game instead of accessing GameLift directly from the game client\. The backend uses the GameLift API operations to initiate matchmaking, request game session placement, and so on\. Popular options for game backend hosting include serverless backends with Amazon API Gateway and AWS Lambda, or container\-based backends with Elastic Load Balancing and Amazon Elastic Container Service \(Amazon ECS\)\. You can store player data in a managed NoSQL database, like those that Amazon DynamoDB provides\. Or, you can use a managed SQL database, like those that Amazon Relational Database Service \(Amazon RDS\) provides\.

The following diagram shows an example game session placement flow\. In this example, fleets are deployed to multiple Regions\. All of the fleets are registered to a single GameLift queue\. For the same result, you could also use a single fleet and define multiple Regional locations for the fleet\.

![\[An example game session placement flow in which three fleets are registered to a single GameLift queue and are deployed to separate AWS Regions.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_hosting_flow.png)