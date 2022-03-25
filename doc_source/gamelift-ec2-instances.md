# Choosing computing resources<a name="gamelift-ec2-instances"></a>

GameLift uses [Amazon Elastic Compute Cloud \(Amazon EC2\) resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html), called **instances**, to deploy your game servers and host game sessions for your players\. When setting up a new fleet, you decide what type of instances your game needs and how to run game server processes on them \(using a runtime configuration\)\. When a fleet is active and ready to host game sessions, you can add or remove instances at any time to accommodate more or fewer players based on demand\. All instances in a fleet use the same type of resources and the same runtime configuration\. You can edit a fleet's runtime configuration and other fleet properties, but the type of resources cannot be changed\.

When choosing resources for a fleet, you need to consider several factors, including game operating system, instance type \(the computing hardware\), and whether to use On\-Demand or Spot Instances\. Keep in mind that hosting costs with GameLift primarily depend on the type of instances you use\. Learn more about [GameLift pricing](https://aws.amazon.com//gamelift/pricing)\.

## Operating systems<a name="gamelift-ec2-instances-os"></a>

GameLift supports game server builds that run on either Microsoft Windows or Amazon Linux \(see [supported game server operating systems](gamelift-supported.md)\)\. When uploading a game build to GameLift, you specify the operating system for the game\. When you create a fleet to deploy the game build, GameLift automatically sets up instances with the build's operating system\.

The cost of resources depends on the operating system in use\. Learn more about the resources available for the supported operating systems: 
+ [Microsoft Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/instance-types.html)
+ [Amazon Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html)

## Instance types<a name="gamelift-ec2-instances-type"></a>

A fleet's instance type determines the kind of hardware that will be used for every instance in the fleet\. Instance types offer different combinations of computing power, memory, storage, and networking capabilities\. With GameLift you have a wide range of instance type options to choose from\. To learn more about the capabilities of each instance type, see [Amazon EC2 instance types](https://aws.amazon.com/ec2/instance-types/)\. 

Consider the geographic locations where you plan to deploy your game servers\. Instance type availability varies by AWS Region\. In addition, check the service limits on the number of instance an AWS account can use, which are applied per instance type, per Region\. You can view a list of available instance types and service limits using the methods described in [Instance service quotas](#gamelift-service-limits)\. 

When choosing from available instance types for your game, consider the following: \(1\) the computing requirements of your game server build, and \(2\) the number of server processes that you plan to run on each instance\. You may be able to run multiple server processes on each instance by using a larger instance type, which can reduce the number of instances you need to meet player demand\. However, larger instance types also cost more\. Learn more about [ running multiple processes on a fleet](fleets-multiprocess.md)\.

## On\-Demand versus Spot Instances<a name="gamelift-ec2-instances-spot"></a>

When creating a new fleet, you designate the fleet type as using either **On\-Demand** or **Spot** Instances\. On\-Demand and Spot Instances offer exactly the same hardware and performance, based on the instance type chosen, and are configured in exactly the same way\. They differ in availability and in cost\. 

**On\-Demand Instances**  
On\-Demand Instances are simply that: you request an instance and it is created for you\. You can always acquire an On\-Demand Instance when you need it and you can keep it as long as you want\. On\-Demand Instances have a fixed cost—you pay for the amount of time that you use them and there are no long\-term commitments\.

**Spot Instances**  
Spot Instances can offer a highly cost\-efficient alternative to On\-Demand Instances by taking advantage of currently unused AWS computing capacity\. The availability of Spot Instances varies, making them less expensive but also generally less viable for game hosting without GameLift\. When using Spot Instances, it is important to understand the impact of these characteristics\. Spot prices fluctuate based on the supply and demand for each instance type in each Region\. Also, Spot Instances can be interrupted by AWS \(with a two\-minute notification\) whenever AWS needs the capacity back\. If GameLift Lift determines that a spot instance is likely to be interrupted, the instance will be put in a recycling state, and GameLift will try to replace it once there are no active game sessions on the instance\.

GameLift provides a set of FleetIQ algorithms to boost the viability of Spot Instances for game hosting\. FleetIQ is used in the game session placement process to find the best available location to place each new game session \(see [Setting up GameLift queues for game session placement](queues-intro.md)\)\. When a queue has Spot fleets, FleetIQ can prioritize low\-cost Spot fleets but eliminates fleets and locations where there's an elevated chance of interruption based historical patterns\. 

You can view pricing history for any instance type in the [GameLift console](https://console.aws.amazon.com/gamelift/)\. The **Spot history** page graphs On\-demand and Spot pricing and calculates the relative cost savings with Spot Instances\. Use the controls to select an instance type, operating system, and a time range\.

You can also evaluate FleetIQ performance using queue metrics, as well as instance\-specific metrics on Spot Instances\. Learn more about [GameLift metrics](monitoring-cloudwatch.md)\. 

Learn more about how to use Spot Instances in the [Using Spot Instances with GameLift](spot-tasks.md)\. 

## Instance service quotas<a name="gamelift-service-limits"></a>

AWS limits the total number of Amazon EC2 instances \(On\-Demand or Spot\) that your AWS account can use with GameLift\. In addition, there's a maximum limits on each instance type, which varies by Region and location\. You can extend these quotas by request\.

You can access information on service quotas using the following methods: 
+ [AWS endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/gamelift.html) lists general quota information for GameLift, as well as other AWS services\.
+ Look up specific quota values for instance types per Region in the [GameLift console](https://console.aws.amazon.com/gamelift/)\. Go to the **Service Limits** page and select a Region to view quotas for the available instance types\. This page also displays current usage values for each instance type in the Region\.
+ Retrieve quota values for instance types per Region with the AWS CLI command [ describe\-ec2\-instance\-limits](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-ec2-instance-limits.html)\. This action also returns the number for instances currently active in the Region\.

For multi\-location fleets, information on instance availability and limits depends on a combination of the fleet's home Region and \(if relevant\) a selected remote location\. Information will be different, for example, for `m5.large` instances that are deployed to the `sa-east-1` Region in \(1\) a fleet that resides in that Region, versus \(2\) a fleet that resides in another Region and deploys instances to `sa-east-1` as a remote location\. 

 