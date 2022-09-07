# Communicate with other AWS resources from your fleets<a name="gamelift-sdk-server-resources"></a>

This topic describes how to set up your game server software to communicate directly and securely with other AWS resources while running on GameLift instances\. You can use this connection to do the following:
+ Send instance log data to Amazon CloudWatch Logs\.
+ Capture CloudWatch metrics for better visibility into instance performance\.
+ Obtain sensitive information, such as passwords, stored remotely in an Amazon Simple Storage Service \(Amazon S3\) bucket\.
+ Dynamically read and write game data, such as game modes or inventory, stored in an Amazon DynamoDB database or other data storage service\.
+ Use Amazon Simple Queue Service \(Amazon SQS\) to send signals directly to an instance\.
+ Access custom resources deployed and running on Amazon Elastic Compute Cloud \(Amazon EC2\)\.

GameLift owns the fleets and instances, but it allocates them to your AWS account\. For your hosted software to interact with AWS resources that you own, you can give limited access permissions to GameLift\. To establish this access, use either of the following methods:
+ \(Recommended\) [Access AWS resources using an IAM role](#gamelift-sdk-server-resources-roles)
+ [Access AWS resources with VPC peering](#gamelift-sdk-server-resources-vpc)

## Access AWS resources using an IAM role<a name="gamelift-sdk-server-resources-roles"></a>

For your game server or other application to access your AWS resources, complete the following steps\.

**To set up access**

1. **Set up an IAM service role for GameLift\.** For instructions on how to set up the AWS Identity and Access Management \(IAM\) service role for GameLift, see [Set up an IAM service role for GameLift](setting-up-role.md)\. After you create the role, copy the role's Amazon Resource Name \(ARN\)\. You use the ARN during fleet creation\.

1. **Associate the service role with a GameLift fleet\.** During fleet creation, provide the service role ARN to the fleet\. Applications that run on any instance in the fleet can then assume the role and acquire the necessary credentials for access\. For instructions on how to create a fleet, see [Deploy a fleet](fleets-creating.md)\.

1. **Add code to your application to assume the service role\.** Any application running on a GameLift instance can assume the associated IAM service role\. This includes game servers and other executables, such as install scripts and daemons\.

   In the application code, before accessing an AWS resource, the application must first assume the service role\. To assume the role, the application must call the AWS Security Token Service \(AWS STS\) `[AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)` API operation and specify the service role ARN\. This operation returns a set of temporary credentials that provide the application with access to the AWS resource\. For more information, see [Using temporary credentials with AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html) in the *IAM User Guide*\.

## Access AWS resources with VPC peering<a name="gamelift-sdk-server-resources-vpc"></a>

You can use Amazon Virtual Private Cloud \(Amazon VPC\) peering to communicate between applications running on a GameLift instance and another AWS resource\. A VPC is a virtual private network that you define that includes a set of resources managed through your AWS account\. Each GameLift fleet has its own VPC\. With VPC peering, you can establish a direct network connection between the VPC for your fleet and the VPC for your other AWS resources\.

GameLift streamlines the process of setting up VPC peering connections for your game servers\. It handles peering requests, updates route tables, and configures the connections as required\. For instructions on how to set up VPC peering for your game servers, see [VPC peering for GameLift](vpc-peering.md)\.