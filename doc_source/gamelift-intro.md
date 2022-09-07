# What is Amazon GameLift?<a name="gamelift-intro"></a>

With Amazon GameLift, you can deploy, operate, and scale dedicated, low\-cost servers in the cloud for session\-based multiplayer games\. Built on AWS global computing infrastructure, GameLift helps deliver high\-performance, high\-reliability game servers while dynamically scaling your resource usage to meet worldwide player demand\.

## Features of GameLift<a name="gamelift-features"></a>

The features and benefits of using GameLift include:
+ Use your own custom multiplayer game servers or use ready\-to\-go Realtime Servers that require minimal configuration and little\-or\-no backend experience\.
+ Provide a low\-latency player experience to support fast\-action gameplay\.
+ Reduce engineering and operational effort to deploy and operate game servers globally\.
+ Get started fast and pay only for what you use, with no upfront costs and no long\-term commitments\.
+ Reduce costs by up to 90 percent with [Amazon Elastic Compute Cloud \(Amazon EC2\)](http://aws.amazon.com/ec2/) Spot Instances\.
+ Use auto scaling to manage your hosting capacity\.
+ Use the GameLift FleetIQ algorithm with your own Amazon EC2 compute resources\.
+ Use GameLift FlexMatch to define matchmaking for your multiplayer games\.

**Tip**  
For more information about GameLift features, including Realtime Servers, [try out the GameLift sample games](gamelift-explore.md)\.

## Get started with GameLift solutions<a name="gamelift-intro-flavors"></a>

GameLift offers a range of solutions for game developers:
+ GameLift hosting for custom\-built game servers
+ GameLift hosting with Realtime Servers
+ GameLift FleetIQ game hosting optimizations with Amazon EC2

### GameLift hosting for custom servers<a name="gamelift-intro-flavors-managed"></a>

GameLift replaces the work required to host your own custom game servers\. Auto scaling capabilities help you avoid paying for more resources than you need\. Auto scaling also helps make sure that you always have games available for new players to join with minimal waiting\.

For more information about GameLift hosting, see [How GameLift works](gamelift-howitworks.md)\.

**Key features**
+ Use GameLift management features, including auto scaling, multi\-location queues, game session placement with the FleetIQ algorithm, game session logging, and metrics\.
+ Deploy game servers to run on Amazon Linux or Windows Server operating systems\.
+ Manage game sessions and player sessions\. Configure game session characteristics, such as the maximum number of players, join rules, and game\-specific properties\.
+ Set up customized health tracking for server processes to detect problems and to resolve poor\-performing processes\.
+ Manage your game resources using AWS CloudFormation templates for GameLift\.

### GameLift hosting with Realtime Servers<a name="gamelift-intro-flavors-realtime"></a>

Use Realtime Servers to stand up games that don't need custom\-built game servers\. This lightweight server solution provides game servers that you can configure to fit your game\.

For more information about GameLift hosting with Realtime Servers, see [How Realtime Servers work](realtime-howitworks.md)\.

**Key features**
+ Use GameLift management features, including auto scaling, multi\-location queues, game session placement with the FleetIQ algorithm, game session logging, and metrics\.
+ Use GameLift hosting resources and choose the type of AWS computing hardware for your fleets\. You can use either Spot Instances or On\-Demand Instances\.
+ Take advantage of a full network stack for game client/server interaction\.
+ Get core game server functionality with customizable server logic\.
+ Make live updates to Realtime configurations and server logic\.
+ Implement FlexMatch matchmaking\.

### GameLift FleetIQ for hosting on Amazon EC2<a name="gamelift-intro-flavors-fleetiq"></a>

With GameLift FleetIQ, you can work directly with your hosting resources in Amazon EC2 and Amazon EC2 Auto Scaling\. This provides the benefit of GameLift optimizations for inexpensive, resilient game hosting\. This solution is for game developers who need more flexibility than fully managed GameLift solutions provide\.

For information about how GameLift FleetIQ works with Amazon EC2 and EC2 Auto Scaling for game hosting, see the [GameLift FleetIQ Developer Guide](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-intro.html)\.

**Key features**
+ Get optimized Spot Instance balancing using the FleetIQ algorithms\.
+ Use player routing features to efficiently manage your game server resources and provide a better player experience for joining games\.
+ Automatically scale hosting capacity based on player usage\.
+ Directly manage EC2 instances in your own AWS account\.
+ Use any of the supported game server executable formats, including Windows, Linux, containers, and Kubernetes\.

## Accessing GameLift<a name="gamelift-intro-access"></a>

Use these tools to work with GameLift\.

**GameLift SDKs**  
The GameLift SDKs contain the libraries needed to communicate with GameLift from your game clients, game servers, and game services\. For more information, see [Download Amazon GameLift SDKs](gamelift-supported.md)\.

**GameLift Realtime Client SDK**  
The Realtime Client SDK enables a game client to connect to theRealtime server, join game sessions, and stay in sync with other players\. Download the [SDK](http://aws.amazon.com/gamelift/getting-started/) and learn more about making API calls with the [Realtime Servers client API \(C\#\)](realtime-sdk-csharp-ref.md)\.

**GameLift console**  
Use the [AWS Management Console for GameLift](https://console.aws.amazon.com/gamelift) to manage your game deployments, configure resources, and track player usage and performance metrics\. The GameLift console provides a GUI alternative to managing resources programmatically with the AWS Command Line Interface \(AWS CLI\)\.

**AWS CLI**  
Use this command line tool to make calls to the AWS SDK, including the GameLift API\. For information about using the AWS CLI, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

## Pricing for GameLift<a name="gamelift-intro-pricing"></a>

GameLift charges for instances by duration of use and for bandwidth by quantity of data transferred\. For a complete list of charges and prices for GameLift, see [Amazon GameLift Pricing](http://aws.amazon.com/gamelift/pricing)\.

For information about calculating the cost of hosting your games or matchmaking with GameLift, see [Generating GameLift pricing estimates](gamelift-calculator.md), which describes how to use the [AWS Pricing Calculator](https://calculator.aws/#/createCalculator/GameLift)\.