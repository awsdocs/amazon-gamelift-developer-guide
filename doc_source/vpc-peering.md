# VPC peering for GameLift<a name="vpc-peering"></a>

This topic provides guidance on how to set up a VPC peering connection between your GameLift\-hosted game servers and your other non\-GameLift resources\. Use Amazon Virtual Private Cloud \(VPC\) peering connections to enable your game servers to communicate directly and privately with your other AWS resources, such as a web service or a repository\. You can establish VPC peering with any resources that run on AWS and are managed by an AWS account that you have access to\.

**Note**  
VPC peering is an advanced feature\. To learn about preferred options for enabling your game servers to communicate directly and privately with your other AWS resources, see [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.

If you're already familiar with Amazon VPCs and VPC peering, understand that setting up peering with GameLift game servers is somewhat different\. You don't have access to the VPC that contains your game servers—it is controlled by the GameLift service—so you can't directly request VPC peering for it\. Instead, you first pre\-authorize the VPC with your non\-GameLift resources to accept a peering request from the GameLift service\. Then you trigger GameLift to request the VPC peering that you just authorized\. GameLift handles the tasks of creating the peering connection, setting up the route tables, and configuring the connection\.

## To set up VPC peering for an existing fleet<a name="vpc-peering-existing"></a>

1. 

**Get AWS account ID\(s\) and credentials\.**

   You need an ID and sign\-in credentials for the following AWS accounts\. You can find AWS account IDs by signing into the [AWS Management Console](https://console.aws.amazon.com/) and viewing your account settings\. To get credentials, go to the IAM console\.
   + AWS account that you use to manage your GameLift game servers\.
   + AWS account that you use to manage your non\-GameLift resources\. 

   If you're using the same account for GameLift and non\-GameLift resources, you need ID and credentials for that account only\.

1. 

**Get identifiers for each VPC\.**

   Get the following information for the two VPCs to be peered: 
   + VPC for your GameLift game servers – This is your GameLift fleet ID\. Your game servers are deployed in GameLift on a fleet of EC2 instances\. A fleet is automatically placed in its own VPC, which is managed by the GameLift service\. You don't have direct access to the VPC, so it is identified by the fleet ID\. 
   + VPC for your non\-GameLift AWS resources – You can establish a VPC peering with any resources that run on AWS and are managed by an AWS account that you have access to\. If you haven't already created a VPC for these resources, see [Getting started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html)\. Once you have created a VPC, you can find the VPC ID by signing into the [AWS Management Console](https://console.aws.amazon.com/) for Amazon VPC and viewing your VPCs\.
**Note**  
When setting up a peering, both VPCs must exist in the same region\. The VPC for your GameLift fleet game servers is in the same region as the fleet\.

1. 

**Authorize a VPC peering\.**

   In this step, you are pre\-authorizing a future request from GameLift to peer the VPC with your game servers with your VPC for non\-GameLift resources\. This action updates the security group for your VPC\.

   To authorize the VPC peering, call the GameLift service API [ CreateVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) or use the AWS CLI command `create-vpc-peering-authorization`\. Make this call using the account that manages your non\-GameLift resources\. Identify the following information:
   + Peer VPC ID – This is for the VPC with your non\-GameLift resources\.
   + GameLift AWS account ID – This is the account that you use to manage your GameLift fleet\. 

   Once you've authorized a VPC peering, the authorization remains valid for 24 hours unless revoked\. You can manage your VPC peering authorizations using the following operations:
   + [DescribeVpcPeeringAuthorizations\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringAuthorizations.html) \(AWS CLI `describe-vpc-peering-authorizations`\)\.
   + [DeleteVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringAuthorization.html) \(AWS CLI `delete-vpc-peering-authorization`\)\.

1. 

**Request a peering connection\.**

   With a valid authorization, you can request that GameLift establish a peering connection\.

   To request a VPC peering, call the GameLift service API [CreateVpcPeeringConnection\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringConnection.html) or use the AWS CLI command `create-vpc-peering-connection`\. Make this call using the account that manages your GameLift game servers\. Use the following information to identify the two VPCs that you want to peer:
   + Peer VPC ID and AWS account ID – This is the VPC for your non\-GameLift resources and the account that you use to manage them\. The VPC ID must match the ID on a valid peering authorization\. 
   + Fleet ID – This identifies the VPC for your GameLift game servers\.

1. 

**Track the peering connection status\.**

   Requesting a VPC peering connection is an asynchronous operation\. To track the status of a peering request and handle success or failure cases, use one of the following options:
   + Continuously poll with `DescribeVpcPeeringConnections()`\. This operation retrieves the VPC peering connection record, including the status of the request\. If a peering connection is successfully created, the connection record also contains a CIDR block of private IP addresses that is assigned to the VPC\.
   + Handle fleet events associated with VPC peering connections with [DescribeFleetEvents\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html), including success and failure events\. 

Once the peering connection is established, you can manage it using the following operations:
+ [DescribeVpcPeeringConnections\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringConnections.html) \(AWS CLI `describe-vpc-peering-connections`\)\.
+ [DeleteVpcPeeringConnection\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringConnection.html) \(AWS CLI `delete-vpc-peering-connection`\)\.

## To set up VPC peering with a new fleet<a name="fleets-creating-aws-cli-vpc"></a>

You can create a new GameLift fleet and request a VPC peering connection at the same time\. 

1. 

**Get AWS account ID\(s\) and credentials\.**

   You need an ID and sign\-in credentials for the following two AWS accounts\. You can find AWS account IDs by signing into the [AWS Management Console](https://console.aws.amazon.com/) and viewing your account settings\. To get credentials, go to the IAM console\.
   + AWS account that you use to manage your GameLift game servers\.
   + AWS account that you use to manage your non\-GameLift resources\. 

   If you're using the same account for GameLift and non\-GameLift resources, you need ID and credentials for that account only\.

1. 

**Get the VPC ID for your non\-GameLift AWS resources\.**

   If you haven't already created a VPC for these resources, do so now \(see [Getting started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html)\)\. Be sure that you create the new VPC in the same region where you plan to create your new fleet\. If your non\-GameLift resources are managed under a different AWS account or user/user group than the one you use with GameLift, you'll need to use these account credentials when requesting authorization in the next step\. 

   Once you have created a VPC, you can locate the VPC ID in Amazon VPC console by viewing your VPCs\.

1. 

**Authorize a VPC peering with non\-GameLift resources\.**

   When GameLift creates the new fleet and a corresponding VPC, it also sends a request to peer with the VPC for your non\-GameLift resources\. You need to pre\-authorize that request\. This step updates the security group for your VPC\.

   Using the account credentials that manage your non\-GameLift resources, call the GameLift service API [ CreateVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) or use the AWS CLI command `create-vpc-peering-authorization`\. Identify the following information:
   + Peer VPC ID – ID of the VPC with your non\-GameLift resources\.
   + GameLift AWS account ID – ID of the account that you use to manage your GameLift fleet\. 

   Once you've authorized a VPC peering, the authorization remains valid for 24 hours unless revoked\. You can manage your VPC peering authorizations using the following operations:
   + [DescribeVpcPeeringAuthorizations\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeVpcPeeringAuthorizations.html) \(AWS CLI `describe-vpc-peering-authorizations`\)\.
   + [DeleteVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteVpcPeeringAuthorization.html) \(AWS CLI `delete-vpc-peering-authorization`\)\.

1. Follow the instructions for [creating a new fleet using the AWS CLI](fleets-creating.md#fleets-creating-aws-cli)\. Include the following additional parameters:
   + *peer\-vpc\-aws\-account\-id* – ID for the account that you use to manage the VPC with your non\-GameLift resources\.
   + *peer\-vpc\-id* – ID of the VPC with your non\-GameLift account\.

A successful call to [create\-fleet](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet.html) with the VPC peering parameters generates both a new fleet and a new VPC peering request\. The fleet's status is set to **New** and the fleet activation process is initiated\. The peering connection request's status is set to **initiating\-request**\. You can track the success or failure of the peering request by calling [describe\-vpc\-peering\-connections](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-vpc-peering-connections.html)\.

When requesting both a new fleet and a VPC peering connection, both actions either succeed or fail\. If a fleet fails during the creation process, the VPC peering connection will not be established\. Likewise, if a VPC peering connection fails for any reason, the new fleet will fail to move from status **Activating** to **Active**\.

**Note**  
The new VPC peering connection is not completed until the fleet is ready to become active\. This means that the connection is not available and can't be used during the game server build installation process\.

The following example creates both a new fleet and a peering connection between a pre\-established VPC and the VPC for the new fleet\. The pre\-established VPC is uniquely identified by the combination of your non\-GameLift AWS account ID and the VPC ID\. 

```
$ AWS gamelift create-fleet
    --name "My_Fleet_1"
    --description "The sample test fleet"
    --ec2-instance-type "c5.large"
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
    --metric-groups  "EMEAfleets"
    --peer-vpc-aws-account-id "111122223333"
    --peer-vpc-id "vpc-a11a11a"
```

*Copyable version:*

```
AWS gamelift create-fleet --name "My_Fleet_1" --description "The sample test fleet" --fleet-type "ON_DEMAND" --metric-groups "EMEAfleets" --build-id "build-1111aaaa-22bb-33cc-44dd-5555eeee66ff" --ec2-instance-type "c5.large" --runtime-configuration "GameSessionActivationTimeoutSeconds=300,MaxConcurrentGameSessionActivations=2,ServerProcesses=[{LaunchPath=C:\game\Bin64.dedicated\MultiplayerSampleProjectLauncher_Server.exe,Parameters=+sv_port 33435 +start_lobby,ConcurrentExecutions=10}]" --new-game-session-protection-policy "FullProtection" --resource-creation-limit-policy "NewGameSessionsPerCreator=3,PolicyPeriodInMinutes=15" --ec2-inbound-permissions "FromPort=33435,ToPort=33435,IpRange=0.0.0.0/0,Protocol=UDP" "FromPort=33235,ToPort=33235,IpRange=0.0.0.0/0,Protocol=UDP" --peer-vpc-aws-account-id "111122223333" --peer-vpc-id "vpc-a11a11a"
```

## Troubleshooting VPC peering issues<a name="vpc-peering-troubleshooting"></a>

If you're having trouble establishing a VPC peering connection for your GameLift game servers, consider these common root causes: 
+ An authorization for the requested connection was not found: 
  + Check the status of a VPC authorization for the non\-GameLift VPC\. It might not exist or it might have expired\.
  + Check the regions of the two VPCs you're trying to peer\. If they're not in the same region, they can't be peered\. 
+ The CIDR blocks \(see [ Invalid VPC peering connection configurations](https://docs.aws.amazon.com/vpc/latest/peering/invalid-peering-configurations.html#overlapping-cidr)\) of your two VPCs are overlapping\. The IPv4 CIDR blocks that are assigned to peered VPCs cannot overlap\. The CIDR block of the VPC for your GameLift fleet is automatically assigned and can't be changed, so you'll need to change the CIDR block for of the VPC for your non\-GameLift resources\. To resolve this issue: 
  + Look up this CIDR block for your GameLift fleet by calling `DescribeVpcPeeringConnections()`\.
  + Go to the Amazon VPC console, find the VPC for your non\-GameLift resources, and change the CIDR block so that they don't overlap\.
+ The new fleet did not activate \(when requesting VPC peering with a new fleet\)\. If the new fleet failed to progress to **Active** status, there is no VPC to peer with, so the peering connection cannot succeed\.