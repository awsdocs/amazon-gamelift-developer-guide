# What is Amazon GameLift?<a name="gamelift-intro"></a>

With Amazon GameLift, you can deploy, operate, and scale dedicated, low\-cost servers in the cloud for session\-based multiplayer games\. Built on AWS global computing infrastructure, GameLift helps deliver high\-performance, high\-reliability game servers while dynamically scaling your resource usage to meet worldwide player demand\.

## Features of GameLift<a name="gamelift-features"></a>

The features and benefits of using GameLift include:
+ Use your own custom multiplayer game servers or use ready\-to\-go Realtime servers that require minimal configuration and little\-or\-no backend experience\.
+ Get started fast and pay only for what you use, with no upfront costs and no long\-term commitments\.
+ Reduce costs by up to 90 percent with [Amazon Elastic Compute Cloud \(Amazon EC2\)](http://aws.amazon.com/ec2/) Spot Instances\.
+ Use auto scaling to manage your hosting capacity\.
+ Use the GameLift FleetIQ algorithm with your own Amazon EC2 compute resources\.
+ Use GameLift FlexMatch to define matchmaking for your multiplayer games\.
+ Use GameLift Anywhere to quickly and iteratively test your game server and client builds\. With GameLift Anywhere, you can use GameLift tools and algorithms with your own hardware\.

**Tip**  
For more information about trying out GameLift features, see [Getting started with Amazon GameLift](getting-started-intro.md)\.

## Get started with GameLift solutions<a name="gamelift-intro-flavors"></a>

**Topics**
+ [GameLift hosting for custom servers](#gamelift-intro-flavors-managed)
+ [GameLift hosting with Realtime Servers](#gamelift-intro-flavors-realtime)
+ [GameLift FleetIQ for hosting on Amazon EC2](#gamelift-intro-flavors-fleetiq)
+ [GameLift FlexMatch for matchmaking](#gamelift-intro-flavors-flexmatch)
+ [GameLift Anywhere hardware hosting](#gamelift-intro-flavors-anywhere)

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

For more information about GameLift hosting with Realtime Servers, see [Integrating games with GameLift Realtime Servers](realtime-intro.md)\.

**Key features**
+ Use GameLift management features, including auto scaling, multi\-location queues, game session placement with the FleetIQ algorithm, game session logging, and metrics\.
+ Use GameLift hosting resources and choose the type of AWS computing hardware for your fleets\. You can use either Spot Instances or On\-Demand Instances\.
+ Take advantage of a full network stack for game client/server interaction\.
+ Get core game server functionality with customizable server logic\.
+ Make live updates to Realtime configurations and server logic\.
+ Implement FlexMatch matchmaking\.

### GameLift FleetIQ for hosting on Amazon EC2<a name="gamelift-intro-flavors-fleetiq"></a>

With GameLift FleetIQ, you can work directly with your hosting resources in Amazon EC2 and Amazon EC2 Auto Scaling\. This provides the benefit of GameLift optimizations for inexpensive, resilient game hosting\. This solution is for game developers who need more flexibility than what fully managed GameLift solutions provide\.

For information about how GameLift FleetIQ works with Amazon EC2 and EC2 Auto Scaling for game hosting, see the [GameLift FleetIQ Developer Guide](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-intro.html)\.

**Key features**
+ Get optimized Spot Instance balancing using the FleetIQ algorithms\.
+ Use player routing features to efficiently manage your game server resources and provide a better player experience for joining games\.
+ Automatically scale hosting capacity based on player usage\.
+ Directly manage EC2 instances in your own AWS account\.
+ Use any of the supported game server executable formats, including Windows, Linux, containers, and Kubernetes\.

### GameLift FlexMatch for matchmaking<a name="gamelift-intro-flavors-flexmatch"></a>

With FlexMatch you can build custom rule sets to define multiplayer matches for your game\. FlexMatch uses rule sets to compare compatible players for each match and provide players with the best possible multiplayer experience\.

For more information about FlexMatch, see [What is Amazon GameLift FlexMatch?](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html)

**Key features**
+ Balance match creation speed and match quality\.
+ Match players or teams based on defined characteristics\.
+ Define rules to place players into matches based on latency\.

### GameLift Anywhere hardware hosting<a name="gamelift-intro-flavors-anywhere"></a>

Use GameLift Anywhere to integrate hardware anywhere in your environment into your GameLift game hosting\. You can integrate Anywhere fleets and EC2 fleets in matchmaker and game session queues to manage matchmaking and game placement across your hardware\.

For more information about testing with Anywhere, see [Test your custom server integration](integration-testing.md)\. For more information about setting up an Anywhere fleet, see [Setting up GameLift fleets](fleets-intro.md)\.

**Key features**
+ Perform fast, iterative testing of your game server and client builds\.
+ Use the set GameLift tools to deploy games to your own hardware\.
+ Use hardware closest to your players, anywhere\.

## Accessing GameLift<a name="gamelift-intro-access"></a>

Use these tools to work with GameLift\.

**GameLift SDKs**  
The GameLift SDKs contain the libraries needed to communicate with GameLift from your game clients, game servers, and game services\. For more information, see [Download Amazon GameLift SDKs](gamelift-supported.md)\.

**GameLift Realtime Client SDK**  
The Realtime Client SDK enables a game client to connect to the Realtime server, join game sessions, and stay in sync with other players\. Download the [SDK](http://aws.amazon.com/gamelift/getting-started/) and learn more about making API calls with the [Realtime Servers client API \(C\#\)](realtime-sdk-csharp-ref.md)\.

**GameLift console**  
Use the [AWS Management Console for GameLift](https://console.aws.amazon.com/gamelift) to manage your game deployments, configure resources, and track player usage and performance metrics\. The GameLift console provides a GUI alternative to managing resources programmatically with the AWS Command Line Interface \(AWS CLI\)\.

**AWS CLI**  
Use this command line tool to make calls to the AWS SDK, including the GameLift API\. For information about using the AWS CLI, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

## Pricing for GameLift<a name="gamelift-intro-pricing"></a>

GameLift charges for instances by duration of use, and for bandwidth by quantity of data transferred\. For a complete list of charges and prices for GameLift, see [Amazon GameLift Pricing](http://aws.amazon.com/gamelift/pricing)\.

For information about calculating the cost of hosting your games or matchmaking with GameLift, see [Generating GameLift pricing estimates](gamelift-calculator.md), which describes how to use the [AWS Pricing Calculator](https://calculator.aws/#/createCalculator/GameLift)\.