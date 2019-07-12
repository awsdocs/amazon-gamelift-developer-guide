# Access AWS Resources From Your Fleets<a name="gamelift-sdk-server-resources"></a>

This topic describes how to set up your game servers and other applications to communicate directly and securely with other AWS resources while running on GameLift instances\. Depending on your game structure, you may want applications or scripts in your game build to communicate with resources on other AWS services\. For example, you might want to:
+ Send instance log data to Amazon CloudWatch logs\.
+ Capture CloudWatch metrics to get better visibility into instance performance\.
+ Obtain sensitive information, such as passwords, that are stored remotely in an Amazon S3 account\.
+ Dynamically read and write game data that is stored in Amazon DynamoDB or other data storage service, such as game modes or inventory\.
+ Use Amazon SQS to send signals directly to an instance\.
+ Access custom resources that are deployed and running on Amazon EC2\.

When you deploy game servers using GameLift, fleets and instances are allocated to your account but they are owned by the GameLift service\. As a result, to enable access to AWS resources that are owned by your AWS account, you need to explicitly extend permissions to the Amazon GameLift service\. In addition, you want to narrowly define the scope of the permissions being extended\. 

There are two potential methods available for securely accessing AWS resources from your hosted applications:
+ Use an AWS Identity and Access Management \(IAM\) role to extend permissions to Amazon GameLift for your resources\. This option is useful to access resources directly associated with AWS services, such as an Amazon S3 bucket, Amazon CloudWatch metrics, or AWS Lambdascripts\. This is the simplest and recommended method\.
+ Use Amazon Virtual Private Cloud \(VPC\) peering connections to allow your Amazon GameLift fleets to communicate securely with AWS resources\. This is an advanced feature and not a commonly used solution\.

## Extend Access to GameLift Using Roles<a name="gamelift-sdk-server-resources-roles"></a>

The following tasks must be completed to enable your game server or other applications to access your AWS resources while running on GameLift instances\. 

1. **Set up an IAM role for the GameLift service\.** Follow the instructions at [Set Up a Role for Amazon GameLift Access](setting-up-role.md) to create an IAM role\. A role lays out a set of permissions for limited access to your AWS resources and specifies the entity \(in this case the GameLift service\) that can assume the role\. Once you've created the role, take note of the new role's Amazon Resource Name \(ARN\), which you'll need when creating a build\.

1. **Associate the service role with a GameLift fleet\.** Once the service role is created, you need to enable your game servers and other applications to assume the service role while running on GameLift instances\. To do this, you must provide the service role ARN to a fleet\. Applications that are running on any instance in the fleet can then assume the role and acquire the necessary credentials for access\. 

   A service role can only be specified during fleet creation\. Once the fleet is created, you cannot add or change the role ARN\. Only one ARN can be provided to a fleet\.

   For help with providing a service role ARN to a fleet, see [Deploy a GameLift Fleet for a Custom Game Build](fleets-creating.md)\.

1. **Add code to your application to assume the service role\.** Any application that is running on a GameLift instance can assume an IAM role, if one is associated with the fleet\. This includes game servers and other executables, such as install scripts and daemons\. An application can access resources during fleet creation and build installation, when starting or stopping service process and game sessions, or in response to game events\. 

   In the application code, before accessing an AWS resource, the application must first assume the service role by calling the AWS Security Token Service API [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) and specifying the same service role ARN that is associated with the fleet\. This action returns a set of temporary credentials, which enable the application to access the AWS resource\. Learn more about [ Using Temporary Security Credentials to Request Access to AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html)\. 

## Use VPC Peering with Amazon GameLift Fleets<a name="gamelift-sdk-server-resources-vpc"></a>

You can use Amazon Virtual Private Cloud \(VPC\) peering to establish fast and secure communication between an application running on a GameLift instance and another AWS resource\. An Amazon VPC is a virtual network, defined by you, that includes a set of resources managed through your AWS account\. Each GameLift fleet has its own VPC\. With VPC peering, you can establish a direct network connection between the VPC for your fleet and the VPC for your other AWS resources\.

For example, you might have a set of web services that support your game, such as for player authentication or social networking\. You can set up a VPC for these resources, and then use GameLift’s VPC peering to enable your game servers to make direct network calls to the web services\. With VPC peering, calls from your game server processes incur minimal latency and, since they are not routed over the public Internet, are not exposed externally\. 

GameLift streamlines the process of setting up VPC peering connections for your game servers\. It handles peering requests, updates route tables, and configures the connections as required\. For more information on how to set up VPC peering for your game servers, see [VPC Peering for Amazon GameLift](vpc-peering.md)\.

See more information on Amazon’s [virtual private clouds](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) and [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)\. You can peer your GameLift fleets with VPCs in any AWS account that you have access to\.