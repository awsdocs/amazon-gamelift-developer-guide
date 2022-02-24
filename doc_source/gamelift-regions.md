# Using GameLift in AWS Regions<a name="gamelift-regions"></a>

GameLift is available in multiple AWS Regions\. For a complete list, see [Amazon GameLift endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/gamelift.html)\. A global AWS account allows you to work with resources in most Regions\.

## GameLift hosting<a name="gamelift-regions-hosting"></a>

GameLift fleets reside in the AWS Region where they are created, which is called the fleet's "home" Region\. The work of managing fleet activity, including auto\-scaling, is done in the home Region\. All fleets deploy their instances in the home Region\.

*Multi\-location fleets* are capable of deploying instances to other Regions in addition to the fleet's home Region\. These fleets are configured with additional "remote" locations\. With multi\-location fleets, you can manage capacity for each location individually, and you can place game sessions by location\. Multi\-location fleets can have remote locations in any AWS Region that GameLift supports, with the exception of China\. These can be created in the following GameLift\-supported AWS Regions: US East \(N\. Virginia\), US West \(Oregon\), EU Central \(Frankfurt\), EU West \(Ireland\), Asia Pacific Southeast \(Sydney\), and Asia Pacific Northeast \(Seoul and Tokyo\)\. If you choose to use a [multi\-location fleet](#gamelift-regions-hosting) with instances in opt\-in Regions, you must enable the Region for your account\. Also, your GameLift administrator policy must allow the `ec2:DescribeRegions` action\. For more information about opt\-in Regions and how to enable them, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\. For a policy example with opt\-in Region support, see [Simple policy examples for administrators](gamelift-iam-policy-examples.md#iam-policy-simple-example)\.

**Important**  
Starting on February 28, 2022, you must enable an opt\-in Region for your AWS account before you can include that Region in a new multi\-location fleet\.  
If you have a multi\-location fleet created before February 28, 2022, and your fleet includes instances in an opt\-in Region that you did not previously enable for your AWS account, those instances will remain unaffected\.
If you create new multi\-location fleets or update existing multi\-location fleets before February 28, 2022, we strongly recommend that you first enable any opt\-in Regions that you choose to use\.

For game session placement, game session queues can be created in any GameLift\-supported Region\. A queue can place new game sessions with fleets in any other Region or location\. The work of game session placement is done in the Region where the queue was created and resides\. Requests for new game sessions that reference a queue are routed to the queue, wherever it resides\.

## GameLift FlexMatch<a name="gamelift-regions-flex"></a>

For FlexMatch, all GameLift\-supported Regions can host match\-generated game sessions\. FlexMatch resources, including matchmaking configurations and rule sets, can be created in the following Regions: US East \(N\. Virginia\), US West \(Oregon\), EU Central \(Frankfurt\), EU West \(Ireland\), Asia Pacific Southeast \(Sydney\), Asia Pacific Northeast \(Seoul and Tokyo\), and China \(Beijing\)\. The work of processing matchmaking requests is done in the Region where the matchmaking configuration and rule set were created and reside\. Requests for matchmaking that reference a matchmaking configuration are routed to it, wherever it resides\.

## GameLift in China<a name="gamelift-regions-china"></a>

When using GameLift for resources in the China \(Beijing\) Region, which is operated by Sinnet, keep in mind that you must have a separate AWS \(China\) account\. In addition, there are some differences in how GameLift is implemented, including how it handles cross\-Region interactions and dependencies\. In particular, multi\-location fleets cannot be supported\. For more information on using GameLift in the China \(Beijing\) Region, see these resources:
+  [Amazon Web Services in China](https://www.amazonaws.cn/en/about-aws/china/)
+  [Amazon GameLift](https://docs.amazonaws.cn/en_us/aws/latest/userguide/gamelift.html) \(Getting Started with Amazon Web Services in China\)