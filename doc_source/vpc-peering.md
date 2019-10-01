# VPC Peering for Amazon GameLift<a name="vpc-peering"></a>

This topic provides guidance on how to set up a VPC peering connection between your GameLift\-hosted game servers and your other non\-GameLift resources\. Use Amazon Virtual Private Cloud \(VPC\) peering connections to enable your game servers to communicate directly and privately with your other AWS resources, such as a web service or a repository\.

**Note**  
VPC peering is an advanced feature\. Learn about alternative options for enabling your game servers to communicate directly and privately with your other AWS resources at [Access AWS Resources From Your Fleets](gamelift-sdk-server-resources.md)\.

If you’re already familiar with Amazon VPCs and VPC peering, please note that setting up peering with GameLift game servers is somewhat different\. You don’t have access to the VPC for your game servers \(it is controlled by the GameLift service\), so you can't directly request VPC peering for it\. Instead, you first pre\-authorize the VPC with your non\-GameLift resources to accept a peering request from the GameLift service\. You then trigger GameLift to request the VPC peering that you just authorized\. GameLift creates the peering connection, sets up the route tables, and configures the connection\.

## Set up VPC Peering for an Existing Fleet<a name="vpc-peering-existing"></a>

**To set up a VPC peering connection:**

1. 

**Get AWS Account ID\(s\) and credentials\.**

   You need an ID and sign\-in credentials for the following AWS accounts\. You can find AWS account IDs by signing into the [AWS Management Console](https://console.aws.amazon.com/) and viewing your account settings\. To get credentials, go to the IAM console\.
   + AWS account that you use to manage your GameLift game servers\.
   + AWS account that you use to manage your non\-GameLift resources\. 

   If you’re using the same account for GameLift and non\-GameLift resources, you need ID and credentials for that account only\.

1. 

**Get identifiers for each VPC\.**

   Get the following information for the two VPCs to be peered: 
   + VPC for your GameLift game servers – This is your GameLift fleet ID\. Your game servers are deployed in GameLift on a fleet of EC2 instances\. A fleet is automatically placed in its own VPC, which is managed by the GameLift service\. You don’t have direct access to the VPC, so it is identified by the fleet ID\. 
   + VPC for your non\-GameLift AWS resources – You can establish a VPC peering with any resources that run on AWS and are managed by an AWS account that you have access to\. If you haven’t already created a VPC for these resources, see [Getting Started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html)\. Once you have created a VPC, you can find the VPC ID by signing into the [AWS Management Console](https://console.aws.amazon.com/) for Amazon VPC and viewing your VPCs\.
**Note**  
When setting up a peering, both VPCs must exist in the same region\. The VPC for your GameLift fleet game servers is in the same region as the fleet\.

1. 

**Authorize a VPC peering with non\-GameLift resources\.**

   In this step, you are pre\-authorizing a future request from GameLift to peer with your VPC for non\-GameLift resources\. This step updates the security group for your VPC\.

   To authorize the VPC peering, call the GameLift service API [ CreateVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) or use the AWS CLI command `create-vpc-peering-authorization`\. Make this call using the account that manages your non\-GameLift resources\. Identify the following information:
   + Peer VPC ID – This is for the VPC with your non\-GameLift resources\.
   + GameLift AWS account ID – This is the account that you use to manage your GameLift fleet\. 

   Once you’ve authorized a VPC peering, the authorization remains valid for 24 hours unless revoked\. You can manage your VPC peering authorizations using the following operations:
   + [DescribeVpcPeeringAuthorizations\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringAuthorizations.html) \(AWS CLI `describe-vpc-peering-authorizations`\)\.
   + [DeleteVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringAuthorization.html) \(AWS CLI `delete-vpc-peering-authorization`\)\.

1. 

**Request a peering between VPCs for a GameLift fleet and the non\-GameLift resources\.**

   With a valid authorization, you can trigger GameLift to request the peering\.

   To request a VPC peering, call the GameLift service API [CreateVpcPeeringConnection\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringConnection.html) or use the AWS CLI command `create-vpc-peering-connection`\. Make this call using the account that manages your GameLift game servers\. Identify the following information, which identifies the two VPCs to peer:
   + Peer VPC ID and AWS account ID – This is the VPC for your non\-GameLift resources and the account that you use to manage them\. The VPC ID used must match one on a valid authorization\. 
   + Fleet ID – This identifies the VPC for your GameLift game servers\.

   You can manage your VPC peering connections using the following operations:
   + [DescribeVpcPeeringConnections\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringConnections.html) \(AWS CLI `describe-vpc-peering-connections`\)\.
   + [DeleteVpcPeeringConnection\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringConnection.html) \(AWS CLI `delete-vpc-peering-connection`\)\.

1. 

**Track your VPC peering connections\.**

   Requesting a VPC peering connection is an asynchronous operation\. To track the status of a peering request and handle success or failure cases, use one of the following options:
   + Continuously poll with `DescribeVpcPeeringConnections()`\. This operation retrieves the VPC peering connection record, including the status of the request\. If a peering connection is successfully created, the connection record also contains a CIDR block of private IP addresses that is assigned to the VPC\.
   + Handle fleet events associated with VPC peering connections with [DescribeFleetEvents\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html), including success and failure events\. 

   Common reasons a connection request fails include:
   + An authorization for the requested connection was not found\. This may mean an existing authorization is no longer valid\. A common cause for this issue is a region mix\-up\. Verify that your authorization and your request are using the same region\.
   + Overlapping CIDR blocks \(see [ Invalid VPC Peering Connection Configurations](https://docs.aws.amazon.com/vpc/latest/peering/invalid-peering-configurations.html#overlapping-cidr)\)\. The IPv4 CIDR blocks that are assigned to peered VPCs cannot overlap\. The CIDR block of the VPC for your GameLift fleet is automatically assigned and can’t be changed\. You can look up this CIDR block by calling `DescribeVpcPeeringConnections()`\. To resolve this issue, you'll need to change the CIDR block of the VPC for your non\-GameLift resources to a non\-overlapping range\.
   + The fleet did not activate\. If you requested a VPC peering connection as part of a `CreateFleet()` request, the new fleet may have failed to progress to **Active** status\. In this scenario, the peering connection cannot succeed\.

## Create a New Fleet with VPC Peering<a name="fleets-creating-aws-cli-vpc"></a>

You can create a new Amazon GameLift fleet and request a VPC peering connection at the same time\. You can establish VPC peering with any resources that run on AWS and are managed by an AWS account that you have access to\. 

If you haven't already set up a VPC for your non\-GameLift resources, 
+ Call the GameLift command [create\-vpc\-peering\-authorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) to pre\-authorize the peering request\. You'll need the ID of the account that your use with GameLift\. This authorization remains valid for 24 hours unless revoked\.

**To create a VPC peering connection with a new fleet:**

1. 

**Get AWS Account ID\(s\) and credentials\.**

   You need an ID and sign\-in credentials for the following two AWS accounts\. You can find AWS account IDs by signing into the [AWS Management Console](https://console.aws.amazon.com/) and viewing your account settings\. To get credentials, go to the IAM console\.
   + AWS account that you use to manage your GameLift game servers\.
   + AWS account that you use to manage your non\-GameLift resources\. 

   If you’re using the same account for GameLift and non\-GameLift resources, you need ID and credentials for that account only\.

1. 

**Get the VPC ID for your non\-GameLift AWS resources\.**

   If you haven’t already created a VPC for these resources, do so now \(see [Getting Started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html)\)\. Be sure that the VPC is in the same region as where you plan to create your new fleet\. When creating the VPC, use the credentials for the account that manages your non\-GameLift resources\. 

   Once you have created a VPC, you can find the VPC ID by signing into the [AWS Management Console](https://console.aws.amazon.com/) for Amazon VPC and viewing your VPCs\.

1. 

**Authorize a VPC peering with non\-GameLift resources\.**

   When GameLift creates the new fleet and VPC, it also sends a request to peer with the VPC for your non\-GameLift resources\. You need to pre\-authorize that request\. This step updates the security group for your VPC\.

   Using the account that manages your non\-GameLift resources, call the GameLift service API [ CreateVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) or use the AWS CLI command `create-vpc-peering-authorization`\. Identify the following information:
   + Peer VPC ID – ID of the VPC with your non\-GameLift resources\.
   + GameLift AWS account ID – ID for the account that you use to manage your GameLift fleet\. 

   Once you’ve authorized a VPC peering, the authorization remains valid for 24 hours unless revoked\. You can manage your VPC peering authorizations using the following operations:
   + [DescribeVpcPeeringAuthorizations\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringAuthorizations.html) \(AWS CLI `describe-vpc-peering-authorizations`\)\.
   + [DeleteVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringAuthorization.html) \(AWS CLI `delete-vpc-peering-authorization`\)\.

1. Follow the instructions for [creating a new fleet using the AWS CLI](fleets-creating.md#fleets-creating-aws-cli)\. Include the following additional parameters:
   + *peer\-vpc\-aws\-account\-id* – ID for the account that you use to manage the VPC with your non\-GameLift resources\.
   + *peer\-vpc\-id* – ID of the VPC with your non\-GameLift account\.

A successful call to [create\-fleet](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet.html) with the VPC peering parameters generates both a new fleet and a new VPC peering request\. The fleet's status is set to **New** and the fleet activation process is initiated\. The peering request's status is set to **initiating\-request**\. You can track the success or failure of the peering request by calling [describe\-vpc\-peering\-connections](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-vpc-peering-connections.html)\.

When requesting a VPC peering connection with a new fleet, both actions either succeed or fail\. If a fleet fails during the creation process, the VPC peering connection will not be established\. Likewise, if a VPC peering connection fails for any reason, the new fleet will fail to move from status **Activating** to **Active**\.

**Note**  
The new VPC peering connection is not completed until the fleet is ready to become active\. As a result, it is not available during installation of the game server build on a new fleet instance\.

The following example shows a request to create both a new fleet and a peering connection between a pre\-established VPC and the VPC that is created for the new fleet\. The pre\-established VPC is uniquely identified by the combination of your non\-GameLift AWS account ID and the VPC ID\. 

```
$ aws gamelift create-fleet
    --name "My_Fleet_1"
    --description "The sample test fleet"
    --ec2-instance-type "c4.large"
    --fleet-type "ON_DEMAND"
    --build-id "build-1111aaaa-22bb-33cc-44dd-5555eeee66ff"
    --runtime-configuration "GameSessionActivationTimeoutSeconds=300,
                             MaxConcurrentGameSessionActivations=2,
                             ServerProcesses=[{LaunchPath=C:\game\Bin64.dedicated\MultiplayerSampleProjectLauncher_Server.exe,
                                               Parameters=+sv_port 33435 +start_lobby,
                                               ConcurrentExecutions=10}]"
    --new-game-session-protection-policy "FullProtection"
    --resource-creation-limit-policy "NewGameSessionsPerCreator=3,
                                      PolicyPeriodInMinutes=15"
    --ec2-inbound-permissions "FromPort=33435,ToPort=33435,IpRange=0.0.0.0/0,Protocol=UDP" 
                              "FromPort=33235,ToPort=33235,IpRange=0.0.0.0/0,Protocol=UDP"
    --MetricGroups "EMEAfleets"
    --peer-vpc-aws-account-id "111122223333"
    --peer-vpc-id "vpc-a11a11a"
```

*Copyable version:*

```
aws gamelift create-fleet --name "My_Fleet_1" --description "The sample test fleet" --fleet-type "ON_DEMAND" --MetricGroups "EMEAfleets" --build-id "build-1111aaaa-22bb-33cc-44dd-5555eeee66ff" --ec2-instance-type "c4.large" --runtime-configuration "GameSessionActivationTimeoutSeconds=300,MaxConcurrentGameSessionActivations=2,ServerProcesses=[{LaunchPath=C:\game\Bin64.dedicated\MultiplayerSampleProjectLauncher_Server.exe,Parameters=+sv_port 33435 +start_lobby,ConcurrentExecutions=10}]" --new-game-session-protection-policy "FullProtection" --resource-creation-limit-policy "NewGameSessionsPerCreator=3,PolicyPeriodInMinutes=15" --ec2-inbound-permissions "FromPort=33435,ToPort=33435,IpRange=0.0.0.0/0,Protocol=UDP" "FromPort=33235,ToPort=33235,IpRange=0.0.0.0/0,Protocol=UDP" --peer-vpc-aws-account-id "111122223333" --peer-vpc-id "vpc-a11a11a"
```