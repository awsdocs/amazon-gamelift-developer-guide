# Choose Computing Resources<a name="gamelift-ec2-instances"></a>

Amazon GameLift uses [Amazon Elastic Compute Cloud \(Amazon EC2\) resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html), called **instances**, to deploy your game servers and host game sessions for your players\. When setting up a new fleet, you decide what type of instances your game needs and how to run game server processes on them \(via the runtime configuration\)\. Once a fleet is active and ready to host game sessions, you can add or remove instances at any time to accommodate more or fewer players\. All instances in a fleet use the same type of resources and the same runtime configuration\. You can edit a fleet's runtime configuration, but the type of resources cannot be changed\.

When choosing resources for a fleet, you'll need to consider several factors, including game operating system, instance type \(the computing hardware\), and whether to use on\-demand or spot instances\. Each of these issues is discussed further in this topic\. Keep in mind that hosting costs with Amazon GameLift primarily depend on the type of resources you use\. Learn more about [Amazon GameLift pricing](https://aws.amazon.com//gamelift/pricing)\.

## Operating Systems<a name="gamelift-ec2-instances-os"></a>

Amazon GameLift supports game server builds that run on either Microsoft Windows or Amazon Linux \(see [supported game server operating systems](gamelift-supported.md)\)\. When uploading a game build to Amazon GameLift, you specify the operating system for the game\. When you create a fleet to deploy the game build, Amazon GameLift automatically sets up instances with the build's operating system\.

The cost of resources depends on the operating system in use\. Learn more about the resources available for the supported operating systems: 
+ [Microsoft Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/instance-types.html)
+ [Amazon Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html)

## Instance Types<a name="gamelift-ec2-instances-type"></a>

A fleet's instance type determines the kind of hardware that will be used for every instance in the fleet\. Instance types offer different combinations of computing power, memory, storage, and networking capabilities\. With Amazon GameLift you have a wide range of instance type options to choose from\. View a [list of available instance types](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-ec2-instance-limits.html) or open the **Service limits** page in the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/) to view instance types, current usage, and usage limits\. To learn more about the capabilities of each instance type, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\. Note that the instance types offered may vary depending on the region\. 

When choosing an instance type for your game, consider the following: \(1\) the computing requirements of your game server build, and \(2\) the number of server processes that you plan to run on each instance\. You may be able to run multiple server processes on each instance by using a larger instance type, which can reduce the number of instances you need to meet player demand\. However, larger instance types also cost more\. Learn more about [ running multiple processes on a fleet](fleets-multiprocess.md)\.

## On\-Demand versus Spot Instances<a name="gamelift-ec2-instances-spot"></a>

When creating a new fleet, you designate the fleet type as using either **on\-demand** or **spot** instances\. On\-demand and spot instances offer exactly the same hardware and performance, based on the instance type chosen, and are configured in exactly the same way\. They differ in available supply and in cost\. 

**On\-demand instances**  
On\-demand instances are simply that: you request an instance and it is created for you\. You can always acquire an on\-demand instance when you need it and keep it as long as you want\. On\-demand instances have a fixed cost; you pay for the amount of time that you use them and there are no long\-term commitments\.

**Spot instances**  
Spot instances offer a highly cost\-efficient alternative to on\-demand instances\. Spot instances use spare AWS computing capacity, so spot instances can cost significantly less than on\-demand instances\. There is one drawback to using spot instances: unlike on\-demand instances, spot prices fluctuate based on the current supply and demand for each instance type in each region\. As a result, spot instances may be interrupted by AWS with a two\-minute notification when AWS needs the capacity back\. 

With Amazon GameLift FleetIQ, however, you can achieve cost savings while maintaining high game\-server availability\. When placing new game sessions, FleetIQ prioritizes the use of spot instances to locate the best available game server based on \(1\) instances with the highest cost savings, and \(2\) instances with historically low interruption rates\.

You can evaluate FleetIQ's performance using a set of queue metrics, as well as instance\- specific metrics on spot instances\. Learn more about [ Amazon GameLift metrics](monitoring-cloudwatch.md)\. You can also view pricing history for any instance type by using the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/)\. The **Spot history** page graphs on\-demand and spot pricing and calculates the relative cost savings with spot\. Use the controls to select an instance type, operating system, and a time range\. 

Learn more about how to use spot instances in the [Spot Fleet Integration Guide](spot-tasks.md)\. 

## Instance Service Limits<a name="gamelift-service-limits"></a>

AWS places limits how many Amazon EC2 instances \(on\-demand or spot\) can be used by an AWS account\. Each instance type has a maximum number allowed per account, and this limit varies by AWS region\. Each account also is limited in the total number of instances used regardless of type of instance\. You can access information on limits in several ways: 
+ Find general limits for Amazon GameLift, as well as all other AWS services on the [AWS Service Limits page\.](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_gamelift)
+ See limits for a specific region in the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/): Select a region and choose **Service limits** from the Amazon GameLift menu\. You can also view the total number of instances currently in use in the region\.
+ Retrieve the maximum number of instances per AWS account \(by region\) by using the Amazon GameLift API action [DescribeEC2InstanceLimits](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeEC2InstanceLimits.html)\. This action also returns the number for instances currently active in the region\.

If you need more instances than allowed by AWS service limits, you can request an increase on the [Amazon GameLift](https://console.aws.amazon.com/gamelift/) Service limits page of the AWS Management Console\. 