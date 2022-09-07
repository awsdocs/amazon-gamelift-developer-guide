# Choosing GameLift computing resources<a name="gamelift-ec2-instances"></a>

To deploy your game servers and host game sessions for your players, GameLift uses [Amazon Elastic Compute Cloud \(Amazon EC2\) resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) called *instances*\. When setting up a new fleet, you decide what type of instances your game needs and how to run game server processes on them\. When a fleet is active and ready to host game sessions, you can add or remove instances at any time to accommodate player demand\. All instances in a fleet use the same type of resources and the same runtime configuration\.

When choosing resources for a fleet, consider the following factors:
+ Game server operating system
+ EC2 instance type
+ On\-Demand Instances or Spot Instances
+ Fleet location

Hosting costs for GameLift primarily depend on the type of instances that you use\. For more information about pricing, see [Amazon GameLift Pricing](http://aws.amazon.com/gamelift/pricing)\.

## Operating systems<a name="gamelift-ec2-instances-os"></a>

GameLift supports game server builds that run on Microsoft Windows or Amazon Linux\. When you upload a game build to GameLift, you specify the operating system for the game\. When you create a fleet to deploy the game build, GameLift automatically sets up instances with the build's operating system\. For more information about supported game server operating systems, see [Download Amazon GameLift SDKs](gamelift-supported.md)\.

The cost of resources depends on the operating system that you use\. For more information about the resources available for the supported operating systems, see the following:
+ [Windows instance types](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/instance-types.html)
+ [Amazon Linux instance types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html)

## Instance types<a name="gamelift-ec2-instances-type"></a>

A fleet's instance type determines the kind of hardware that the instances in the fleet use\. Different instance types offer different combinations of computing power, memory, storage, and networking capabilities\. For more information about each instance type, see [Amazon EC2 Instance Types](http://aws.amazon.com/ec2/instance-types/)\.

When choosing from available instance types for your game, consider:
+ The computing requirements of your game server build\.
+ The number of server processes that you plan to run per instance\.

By using a larger instance type, you may be able to run multiple server processes on each instance\. This can reduce the number of instances required to meet player demand\. For more information about running multiple processes per instance, see [Managing how game servers are launched for hosting](fleets-multiprocess.md)\.

## On\-Demand Instances versus Spot Instances<a name="gamelift-ec2-instances-spot"></a>

Amazon EC2 On\-Demand Instances and Spot Instances offer the same hardware and performance, but they differ in availability and cost\.

**On\-Demand Instances**  
You can always acquire an On\-Demand Instance when you need it, and you can keep it as long as you want\. On\-Demand Instances have a fixed cost; you pay for the amount of time that you use them, and there are no long\-term commitments\.

**Spot Instances**  
Spot Instances can offer a highly cost\-efficient alternative to On\-Demand Instances by taking advantage of unused AWS computing capacity\. The availability of Spot Instances varies, making them less expensive but less viable for game hosting without GameLift\. Spot Instance prices fluctuate based on the supply and demand for each instance type in each location\. Also, AWS can interrupt Spot Instances \(with a two\-minute notification\) whenever it needs the capacity back\. If GameLift determines that a Spot Instance may be interrupted, it puts the instance in a recycling state\. Then, when there are no active game sessions on the instance, GameLift tries to replace it\.

To boost the viability of Spot Instances for game hosting, GameLift provides a the FleetIQ algorithm\. The FleetIQ algorithm finds the best available location to place new game sessions\. For more information about using queues with the FleetIQ algorithm, see [Setting up GameLift queues for game session placement](queues-intro.md)\.

For more information about how to use Spot Instances, see [Using Spot Instances with GameLift](spot-tasks.md)\.

## Fleet location<a name="gamelift-ec2-instances-location"></a>

Consider the geographic locations where you plan to deploy your game servers, as instance type availability varies by AWS Region and Local Zone\. Check the service quotas for your AWS account to determine the amount of each instance type that you can use, per Region or Local Zone\. For instructions about viewing your available instance types and service quotas, see [Instance service quotas](#gamelift-service-limits)\.

For multi\-location fleets, instance availability and quotas depend on a combination of the fleet's home Region and selected remote locations\. For more information about fleet locations, see [GameLift hosting in AWS Regions and Local Zones](gamelift-regions.md)\.

## Instance service quotas<a name="gamelift-service-limits"></a>

AWS limits the total number of Amazon EC2 instances that your AWS account can use with GameLift\. There is also a quota for each instance type, which varies by location\. If you need more capacity than a default quota provides, you can [request a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html)\.

To see the default service quotas for GameLift and the current quotas for your AWS account, do the following:
+ For general service quota information for GameLift, see [Amazon GameLift endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/gamelift.html) in the *AWS General Reference*\.
+ For a list of available instance types per location for your account, open the [Service quotas](https://console.aws.amazon.com/gamelift/service-quotas) page of the GameLift console\. This page also displays your account's current usage for each instance type in each location\.
+ For a list of your account's current quotas for instance types per Region, run the AWS Command Line Interface \(AWS CLI\) command [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/describe-ec2-instance-limits.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/describe-ec2-instance-limits.html)\. This command also returns the number of active instances that you have in your default Region \(or in another Region that you specify\)\.