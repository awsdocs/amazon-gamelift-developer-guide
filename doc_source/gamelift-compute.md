# Choosing GameLift compute resources<a name="gamelift-compute"></a>

To deploy your game servers and host game sessions for your players, Amazon GameLift uses [Amazon Elastic Compute Cloud \(Amazon EC2\) resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) called *instances*, or your physical hardware\. When setting up a new fleet using instances, decide what type of instances you need and how to run game server processes on them\. When an managed EC2 fleet is active and ready to host game sessions, you can add or remove instances as needed to accommodate player demand\.

You can deploy your GameLift game servers on a combination of two compute types:
+ **Managed EC2** – Managed EC2 fleets use Amazon EC2 instances to host your game servers\. GameLift manages the instances and removes the burden of hardware and software management from hosting your games\.
+ **GameLift Anywhere** – GameLift Anywhere fleets use your existing infrastructure to host game servers while GameLift manages your matchmaking and queues\.

**Topics**
+ [Available hardware](#gamelift-compute-hardware)
+ [Fleet location](#gamelift-compute-location)
+ [On\-Demand Instances versus Spot Instances](#gamelift-compute-spot)
+ [Operating systems](#gamelift-compute-os)
+ [Instance types](#gamelift-compute-instance)
+ [Service quotas](#gamelift-service-limits)

## Available hardware<a name="gamelift-compute-hardware"></a>

Consider the existing infrastructure in your implementation\. While you migrate games to GameLift, you can continue to use your infrastructure\. With GameLift Anywhere, you can use your own infrastructure along with GameLift managed EC2 instances\. You can also use your existing infrastructure to host games closer to your players than supported GameLift locations can allow\. For more information about setting up GameLift Anywhere fleets, see [Create a GameLift Anywhere fleet](fleets-creating-anywhere.md)\.

## Fleet location<a name="gamelift-compute-location"></a>

Consider the geographic locations where you plan to deploy your game servers\. Instance type availability varies by AWS Region and Local Zone\.

For multi\-location fleets, instance availability and quotas depend on a combination of the fleet's home Region and selected remote locations\. For more information about fleet locations, see [GameLift hosting in AWS Regions and Local Zones](gamelift-regions.md)\.

For GameLift Anywhere fleets, you determine the location of your physical hardware\. For more information about custom locations, see [GameLift Anywhere](gamelift-regions.md#gamelift-regions-anywhere)\.

## On\-Demand Instances versus Spot Instances<a name="gamelift-compute-spot"></a>

Amazon EC2 On\-Demand Instances and Spot Instances offer the same hardware and performance, but they differ in availability and cost\.

**On\-Demand Instances**  
You can always acquire an On\-Demand Instance when you need it, and keep it for as long as you want\. On\-Demand Instances have a fixed cost, meaning you pay for the amount of time that you use them, and there are no long\-term commitments\.

**Spot Instances**  
Spot Instances can offer a cost\-efficient alternative to On\-Demand Instances by utilizing unused AWS computing capacity\. Spot Instance prices fluctuate based on the supply and demand for each instance type in each location\. AWS can interrupt Spot Instances whenever it needs the capacity back\. If GameLift uses the FleetIQ algorithm to determine that a Spot Instance might be interrupted, it puts the instance in a recycling state\. Then, when there are no active game sessions on the instance, GameLift tries to replace it\.

The FleetIQ algorithm finds a suitable available location to place new game sessions\. For information about using queues with the FleetIQ algorithm, see [Setting up GameLift queues for game session placement](queues-intro.md)\.

For more information about how to use Spot Instances, see [Use Spot Instances with GameLift](spot-tasks.md)\.

## Operating systems<a name="gamelift-compute-os"></a>

GameLift instances support game server builds that run on Microsoft Windows or Amazon Linux\. When you upload a game build to GameLift, specify the operating system for the game\. When you create an Amazon EC2 fleet to deploy the game build, GameLift automatically sets up instances with the build's operating system\. For more information about supported game server operating systems, see [Download Amazon GameLift SDKs](gamelift-supported.md)\.

When using a GameLift Anywhere fleet, you can use any operating system that your hardware supports\. GameLift Anywhere fleets require you to deploy your game build to the hardware while using GameLift to manage your resources in one place\.

## Instance types<a name="gamelift-compute-instance"></a>

An Amazon EC2 fleet's instance type determines the kind of hardware that the instances use\. Different instance types offer different combinations of computing power, memory, storage, and networking capabilities\. For more information about each instance type, see [Amazon EC2 Instance Types](http://aws.amazon.com/ec2/instance-types/)\.

When choosing from available instance types for your game, consider:
+ The computing requirements of your game server build\.
+ The number of server processes that you plan to run per instance\.

By using a larger instance type, you may be able to run multiple server processes on each instance\. This can reduce the number of instances required to meet player demand\. For more information about running multiple processes per instance, see [Manage how GameLift launches game servers](fleets-multiprocess.md)\.

## Service quotas<a name="gamelift-service-limits"></a>

To see the default service quotas for GameLift, and the current quotas for your AWS account, do the following:
+ For general service quota information for GameLift, see [Amazon GameLift endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/gamelift.html) in the *AWS General Reference*\.
+ For a list of available instance types per location for your account, open the [Service quotas](https://console.aws.amazon.com/gamelift/service-quotas) page of the GameLift console\. This page also displays your account's current usage for each instance type in each location\.
+ For a list of your account's current quotas for instance types per Region, run the AWS Command Line Interface \(AWS CLI\) command [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/describe-ec2-instance-limits.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/describe-ec2-instance-limits.html)\. This command returns the number of active instances that you have in your default Region \(or in another Region that you specify\)\.

As you prepare to launch you game, fill out a launch questionnaire in the [GameLift console](https://console.aws.amazon.com/gamelift/)\. The GameLift team uses the launch questionnaire to determine the correct quotas and limits for your game\.