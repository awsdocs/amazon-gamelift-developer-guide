# Hosting components overview<a name="gamelift_quickstart_customservers_overview"></a>

This section provides a high\-level overview of the different components in a custom server\. It also provides an example game session placement flow\. 

**GameLift FlexMatch matchmaking configuration and rule set** — If you want to use the matchmaking capabilities of GameLift, you must define a matchmaking configuration, provide a JSON\-document based rule set, and use the matchmaking APIs of GameLift from your backend service\. 

**GameLift fleets** — A fleet contains EC2 instances hosted by GameLift\. A fleet uses the configuration and scaling logic that you define to run your game server build\. A fleet can be used directly without a queue\. Multiple fleets can be associated with a GameLift queue\. A common pattern is to use Spot Fleets configured with your preferred locations, and a backup on\-demand fleet with the same locations\. Using multiple Spot Fleets of different instance types reduces the chance of on\-demand placement\. 

**GameLift queue** — To place game sessions across GameLift fleets globally, you must create a GameLift queue\. The queue contains a prioritized order of your fleets as well as your latency requirements\. The queue places game sessions on the lowest cost and lowest latency fleet\. With the introduction of multi\-Region fleets, you do not need to separate fleets per Region\. The queue can manage placement across multiple locations within a single fleet\. 

One fleet can span across GameLift supported Regions\. You don't need to run different fleets in all the different Regions when the fleets don't need custom configuration or builds\. Since fleets are managed in your home Region, we recommend that you have fleets in two different home Regions for redundancy\. 

**Game backend service** — In addition to GameLift, you always need to host a backend service for your game instead of accessing GameLift directly from the game client\. The backend uses the GameLift APIs to initiate matchmaking, request game session placement, and so on\. Popular options for game backend hosting include serverless backends with Amazon API Gateway and AWS Lambda, or container\-based backends with Elastic Load Balancing and Amazon Elastic Container Service\. You can store player data in a managed NoSQL database like Amazon DynamoDB or a managed SQL database like Amazon Relational Database Service\. 

The following diagram shows an example game session placement flow\. In the diagram, fleets are deployed to multiple Regions\. All of the fleets are registered to a single GameLift queue\. You can also use a single fleet and define multiple Regional locations for the fleet for the same result\. 

![\[Hosting component flow\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_hosting_flow.png)