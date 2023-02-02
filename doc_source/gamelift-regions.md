# GameLift hosting in AWS Regions and Local Zones<a name="gamelift-regions"></a>

GameLift is available in multiple AWS Regions and Local Zones\. For a complete list of locations, see [Amazon GameLift endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/gamelift.html) in the *AWS General Reference*\.

## GameLift hosting<a name="gamelift-regions-hosting"></a>

When you create a GameLift fleet, GameLift creates the fleet's resources in your current AWS Region\. GameLift calls this Region the fleet's *home Region*\. To manage a fleet, access it from its home Region\.

*Multi\-location fleets* deploy instances to other locations in addition to the fleet's home Region\. With multi\-location fleets, you can manage capacity for each location individually, and you can place game sessions by location\. Multi\-location fleets can have remote locations in any Region or Local Zone that GameLift supports\. The following diagram depicts a multi\-location fleet with resources in two Regions\. In the diagram, the `us-west-2` Region includes two game servers, and the `us-east-2` Region has one game server\.

![\[A multi-location GameLift fleet in two AWS Regions, each with their own game server resources.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/fleet_multi_location.png)

If you choose to use a [multi\-location fleet](#gamelift-regions-hosting) with instances in Regions that aren't enabled by default, you must enable those Regions in your AWS account\. Also, your GameLift administrator policy must allow the `ec2:DescribeRegions` action\. For more information about Regions that aren't enabled by default and how to enable them, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\. For a policy example with Regions that aren't enabled by default, see [Administrator policy examples](gamelift-iam-policy-examples.md#iam-policy-simple-example)\.

**Important**  
To use Regions that aren't enabled by default, enable them in your AWS account\.  
Fleets with Regions that aren't enabled that you created before February 28, 2022 are unaffected\.
To create new multi\-location fleets or to update existing multi\-location fleets, first enable any Regions that you choose to use\.

For game session placement, you can create game session queues in any location that GameLift supports\. GameLift places game sessions from the location where you created the queue\.

## Local Zones<a name="gamelift-regions-local-zones"></a>

A *Local Zone* is an extension of an AWS Region in geographic proximity to your users\. Local Zones have their own connections to the internet\. Local Zones also support AWS Direct Connect so that resources created in a Local Zone can serve local users with low\-latency communications\. For more information, see [AWS Local Zones](http://aws.amazon.com/about-aws/global-infrastructure/localzones/)\.

The code for a Local Zone is its Region code, followed by an identifier that indicates its physical location\. For example, the `us-west-2-lax-1` Local Zone is in Los Angeles\. For a list of available Local Zones, see [Available Local Zones](#gamelift-regions-local-zones-availability)\.

GameLift hosts your games in each of the locations that you choose for your fleet\. The following diagram depicts a fleet with two game servers in the `us-west-2` Region, one game server in the `us-east-2` Region, and one game server in the `us-west-2-lax-1` Local Zone\.

![\[A GameLift fleet that spans the us-west-2 and us-east-2 Regions, and the us-west-2-lax-1 Local Zone.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/fleet_local_zones.png)

### Available Local Zones<a name="gamelift-regions-local-zones-availability"></a>

The following table lists the available Local Zones and their physical locations\.


| Local Zone | Location \(metro area\) | 
| --- | --- | 
|  `us-east-1-atl-1`  |  Atlanta  | 
|  `us-east-1-chi-1`  |  Chicago  | 
|  `us-east-1-dfw-1`  |  Dallas  | 
|  `us-east-1-iah-1`  |  Houston  | 
|  `us-east-1-mci-1`  |  Kansas City  | 
|  `us-west-2-den-1`  |  Denver  | 
|  `us-west-2-lax-1`  | Los Angeles | 
|  `us-west-2-phx-1`  | Phoenix | 

## GameLift Anywhere<a name="gamelift-regions-anywhere"></a>

You can use GameLift Anywhere to create fleets with your own hardware, and manage your game builds, scripts, game servers, and clients using GameLift\. You can manage your GameLift Anywhere resources from the you created the Anywhere fleet\. GameLift Anywhere is available in all Regions that GameLift supports\. For more information about creating an Anywhere fleet and testing your game server integration, see [Create a GameLift Anywhere fleet](fleets-creating-anywhere.md) and [Test your custom server integration](integration-testing.md)\.

With GameLift Anywhere you create custom locations that represent the physical location of the hardware you are using to host your GameLift integrated game servers\.

## GameLift FlexMatch<a name="gamelift-regions-flex"></a>

For FlexMatch, all locations that GameLift supports can host match\-generated game sessions\. GameLift processes matchmaking requests in the Region where you create the matchmaking configuration\. For requests that reference a matchmaking configuration, GameLift routes those requests to the Region where you created the configuration, wherever it resides\. For more information about GameLift FlexMatch, see [What is Amazon GameLift FlexMatch?](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html)

## GameLift in China<a name="gamelift-regions-china"></a>

When using GameLift for resources in the China \(Beijing\) Region, operated by Sinnet, or the China \(Ningxia\) Region, operated by NWCD, you must have a separate AWS \(China\) account\. Note that some features are unavailable in the China Regions\. For more information about using GameLift in these Regions, see the following resources:
+  [Amazon Web Services in China](https://www.amazonaws.cn/en/about-aws/china/)
+  [Amazon GameLift](https://docs.amazonaws.cn/en_us/aws/latest/userguide/gamelift.html) \(Getting Started with Amazon Web Services in China\)