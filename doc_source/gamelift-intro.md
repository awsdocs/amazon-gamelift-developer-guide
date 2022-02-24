# What Is Amazon GameLift?<a name="gamelift-intro"></a>

Amazon GameLift enables developers to deploy, operate, and scale dedicated, low\-cost servers in the cloud for session\-based, multiplayer games\. Built on AWS global computing infrastructure, GameLift helps deliver high\-performance, high\-reliability, low\-cost game servers while dynamically scaling your resource usage to meet worldwide player demand\. 

## Why GameLift ?<a name="why-gamelift"></a>

Here are some of the benefits of using Amazon GameLift:
+ Bring your own fully custom multiplayer game servers or use ready\-to\-go Realtime Servers that require minimal configuration and little or no backend experience\.
+ Provide low\-latency player experience to support fast\-action game play\.
+ Enhance your matchmaking services with intelligent queuing, game session placement, and match backfill\.
+ Reduce engineering and operational effort to deploy and operate game servers globally\.
+ Get started fast and pay as you go, with no upfront costs and no long\-term commitments\.
+ Reduce costs by up to 90% with Spot Instances\.
+ Rely on Amazon Web Services \(AWS\), including [Amazon Elastic Compute Cloud \(Amazon EC2\)](https://aws.amazon.com/ec2/) for web\-scale cloud computing resources and auto\-scaling to manage your hosting capacity\.

**Tip**  
[Learn more about ways to explore Amazon GameLift features, including Realtime Servers, using sample games](gamelift-explore.md)\.

## GameLift solutions<a name="gamelift-intro-flavors"></a>

GameLift offers a range of solutions for game developers:
+ GameLift hosting for custom\-built game servers\.
+ GameLift hosting with Realtime Servers
+ GameLift FleetIQ game hosting optimizations for use with Amazon EC2 

### GameLift hosting<a name="gamelift-intro-flavors-managed"></a>

Amazon GameLift offers a fully managed service for deploying, operating, and scaling session\-based, multiplayer game servers\. GameLift replaces the work required to host your own custom game servers, including buying and setting up hardware, and managing ongoing activity, security, storage, and performance tracking\. Auto\-scaling capabilities provide additional protection from having to pay for more resources than you need, while making sure you always have games available for new players to join with minimal waiting\.

To learn more about how the GameLift hosting solution works, see [How GameLift works](gamelift-howitworks.md)\.

**Key features**
+ Provide high\-quality game hosting to players around the world by deploying computing resources in multiple AWS Regions\.
+ Deploy game servers to run on Amazon Linux or Windows Server operating systems\.
+ Let FleetIQ optimize the use of low\-cost Spot Instances\. On their own, Spot Instances are not always viable for game hosting due to potential interruptions\. FleetIQ prediction algorithms find the Spot Instances that are best suited to host new game sessions\.
+ Use automatic scaling tools to adjust your game hosting capacity to meet actual player demand\. These tools allow you to keep hosting costs in line while maintaining enough capacity to get new players into games fast\.
+ Build a custom matchmaking service for your game using FlexMatch\. Create single\-team or multi\-team matches for up to 200 players\.
+ Manage game sessions and player sessions\. Configure game session characteristics, such as maximum number of players allowed, join rules, and game\-specific properties\.
+ Choose from a range of options to help players find suitable game sessions\. Use GameLift Queues to intelligently place new game sessions across multiple regions, provide players with filtered and sorted lists of available game sessions \("list and pick"\), or implement a full matchmaking system with FlexMatch\. 
+ Analyze game performance using the Amazon GameLift console to track metrics, view game session logs, and review data on individual game sessions and player sessions\.
+ Set up customized health tracking for server processes to detect problems fast and resolve poor\-performing processes\.
+ Manage your game resources using AWS CloudFormation templates for GameLift\.

### GameLift hosting with Realtime Servers<a name="gamelift-intro-flavors-realtime"></a>

Use Realtime Servers to stand up games that don't need custom\-built game servers\. This lightweight server solution provides ready\-to\-go game servers that can be configured to fit your game\. You can deploy game servers with anything from minimal configuration settings to custom logic that is specific to your game and players\. 

To learn more about how GameLift hosting with Realtime Servers works, see [How Realtime Servers Work](realtime-howitworks.md)\.

**Key features**
+ Use GameLift management features, including auto\-scaling, multi\-region queues, game session placement with FleetIQ, game session logging, and metrics\. 
+ Use GameLift hosting resources, choose the type of AWS computing hardware for your fleets\. Use either Spot or On\-Demand Instances\.
+ Take advantage of a full network stack for game client/server interaction\.
+ Get core game server functionality with customizable server logic\. 
+ Make live updates to Realtime configurations and server logic\. Update your Realtime server configuration at any time\. 
+ Implement FlexMatch matchmaking\. 

### GameLift FleetIQ for hosting on Amazon EC2<a name="gamelift-intro-flavors-fleetiq"></a>

GameLift FleetIQ optimizes the use of low\-cost Spot Instances for cloud\-based game hosting\. With this feature, you can work directly with your hosting resources in Amazon EC2 and Auto Scaling and take advantage of GameLift optimizations to deliver inexpensive, resilient game hosting for your players\. This solution is designed for game developers who need more flexibility than is offered in the fully managed GameLift solutions\. 

To learn more about how GameLift FleetIQ works with Amazon EC2 and Auto Scaling for game hosting, see the [ GameLift FleetIQ Guide](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-intro.html)\.

**Key features**
+ Get optimized Spot balancing using GameLift FleetIQ prediction algorithms\.
+ Use player routing features to efficiently manage your game server resources and provide optimal player experience when joining games\.
+ Automatically scale hosting capacity based on player usage\. 
+ Directly manage Amazon EC2 instances in your own AWS account\.
+ Use any of multiple supported game server executable formats, including Windows, Linux, containers, and Kubernetes\.
+ Choose from multiple types of Amazon EC2 computing resources\. 
+ Reach players worldwide by deploying across 15 Regions, including China\.