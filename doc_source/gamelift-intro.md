# What is Amazon GameLift?<a name="gamelift-intro"></a>

With Amazon GameLift, you can deploy, operate, and scale dedicated, low\-cost servers in the cloud for session\-based multiplayer games\. Built on AWS global computing infrastructure, GameLift helps deliver high\-performance, high\-reliability game servers while dynamically scaling your resource usage to meet worldwide player demand\.

## Why GameLift?<a name="why-gamelift"></a>

Here are some of the benefits of using GameLift:
+ Use your own custom multiplayer game servers or use ready\-to\-go Realtime Servers that require minimal configuration and little\-or\-no backend experience\.
+ Provide a low\-latency player experience to support fast\-action gameplay\.
+ Enhance your matchmaking services with intelligent queuing, game session placement, and match backfill\.
+ Reduce engineering and operational effort to deploy and operate game servers globally\.
+ Get started fast and pay as you go, with no upfront costs and no long\-term commitments\.
+ Reduce costs by up to 90% with [Amazon Elastic Compute Cloud \(Amazon EC2\)](https://aws.amazon.com/ec2/) Spot Instances\.
+ Rely on AWS, including Amazon EC2, for web\-scale cloud computing resources and auto\-scaling to manage your hosting capacity\.

**Tip**  
To learn more about GameLift features, including Realtime Servers, [try out the GameLift sample games](gamelift-explore.md)\.

## GameLift solutions<a name="gamelift-intro-flavors"></a>

GameLift offers a range of solutions for game developers:
+ GameLift hosting for custom\-built game servers
+ GameLift hosting with Realtime Servers
+ GameLift FleetIQ game hosting optimizations with Amazon EC2 

### GameLift hosting for custom servers<a name="gamelift-intro-flavors-managed"></a>

GameLift replaces the work required to host your own custom game servers, such as buying and setting up hardware, and managing ongoing activity, security, storage, and performance tracking\. Auto\-scaling capabilities help you avoid paying for more resources than you need, while making sure that you always have games available for new players to join with minimal waiting\.

For more information about GameLift hosting, see [How GameLift works](gamelift-howitworks.md)\.

**Key features**
+ Provide high\-quality game hosting to players around the world by deploying computing resources in multiple AWS Regions\.
+ Deploy game servers to run on Amazon Linux or Windows Server operating systems\.
+ Let GameLift FleetIQ optimize the use of low\-cost Amazon EC2 Spot Instances\. On their own, Spot Instances are not always viable for game hosting due to potential interruptions\. FleetIQ prediction algorithms find the Spot Instances that are best suited to host new game sessions\.
+ Use auto\-scaling tools to adjust your game hosting capacity to meet actual player demand\. You can use these tools to keep hosting costs in line while maintaining enough capacity to get new players into games quickly\.
+ Build a custom matchmaking service for your game using GameLift FlexMatch\. Create single\-team or multi\-team matches for up to 200 players\.
+ Manage game sessions and player sessions\. Configure game session characteristics, such as the maximum number of players, join rules, and game\-specific properties\.
+ Choose from a range of options to help players find suitable game sessions\. Use GameLift queues to intelligently place new game sessions across multiple Regions, provide players with filtered and sorted lists of available game sessions \("list and pick"\), or implement a full matchmaking system with FlexMatch\.
+ Analyze game performance by using the GameLift console to track metrics, view game session logs, and review data on individual game sessions and player sessions\.
+ Set up customized health tracking for server processes to detect problems quickly and to resolve poor\-performing processes\.
+ Manage your game resources using AWS CloudFormation templates for GameLift\.

### GameLift hosting with Realtime Servers<a name="gamelift-intro-flavors-realtime"></a>

Use Realtime Servers to stand up games that don't need custom\-built game servers\. This lightweight server solution provides ready\-to\-go game servers that you can configure to fit your game\. You can deploy game servers with anything from minimal configuration settings to custom logic that is specific to your game and players\.

For more information about GameLift hosting with Realtime Servers, see [How Realtime Servers work](realtime-howitworks.md)\.

**Key features**
+ Use GameLift management features, including auto\-scaling, multi\-Region queues, game session placement with FleetIQ, game session logging, and metrics\.
+ Use GameLift hosting resources and choose the type of AWS computing hardware for your fleets\. You can use either Spot Instances or On\-Demand Instances\.
+ Take advantage of a full network stack for game client/server interaction\.
+ Get core game server functionality with customizable server logic\.
+ Make live updates to Realtime configurations and server logic\. Update your Realtime server configuration at any time\.
+ Implement FlexMatch matchmaking\.

### GameLift FleetIQ for hosting on Amazon EC2<a name="gamelift-intro-flavors-fleetiq"></a>

GameLift FleetIQ optimizes the use of low\-cost Spot Instances for cloud\-based game hosting\. With this feature, you can work directly with your hosting resources in Amazon EC2 and Amazon EC2 Auto Scaling with the benefit of GameLift optimizations for inexpensive, resilient game hosting\. This solution is designed for game developers who need more flexibility than fully managed GameLift solutions provide\.

For information about how GameLift FleetIQ works with Amazon EC2 and EC2 Auto Scaling for game hosting, see the [GameLift FleetIQ Developer Guide](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-intro.html)\.

**Key features**
+ Get optimized Spot balancing using FleetIQ prediction algorithms\.
+ Use player routing features to efficiently manage your game server resources and provide the optimum player experience for joining games\.
+ Automatically scale hosting capacity based on player usage\.
+ Directly manage EC2 instances in your own AWS account\.
+ Use any of multiple supported game server executable formats, including Windows, Linux, containers, and Kubernetes\.
+ Choose from multiple types of Amazon EC2 computing resources\.
+ Reach players worldwide by deploying across 15 Regions, including in China\.

## Pricing for GameLift<a name="gamelift-intro-pricing"></a>

GameLift charges for instances by duration of use and for bandwidth by quantity of data transferred\. For a complete list of charges and prices for GameLift, see [Amazon GameLift Pricing](http://aws.amazon.com/gamelift/pricing)\.

For information on calculating the cost of hosting your games or matchmaking with GameLift, see [Generating GameLift pricing estimates](gamelift-calculator.md), which describes how to use the [AWS Pricing Calculator](https://calculator.aws/#/createCalculator/GameLift)\.