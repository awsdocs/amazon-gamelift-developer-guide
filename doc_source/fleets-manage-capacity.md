# Scaling GameLift hosting capacity<a name="fleets-manage-capacity"></a>

Hosting capacity, measured in instances, represents the number of game sessions that GameLift can host concurrently and the number of concurrent players that those game sessions can accommodate\. One of the most challenging tasks with game hosting is scaling capacity to meet player demand without wasting money on resources that you don't need\. For more information, see [Scaling fleet capacity](gamelift-howitworks.md#gamelift-howitworks-capacity)\.

Capacity is adjusted at the fleet location level\. All fleets have at least one location: the fleet's home AWS Region\. When viewing or scaling capacity, the information is listed by location, including the fleet's home Region and any additional remote locations\.

You can manually set the number of instances to maintain, or you can set up auto scaling to dynamically adjust capacity as player demand changes\. We recommend that you start by turning on the target\-based auto scaling option\. The goal of target\-based auto scaling is to maintain enough hosting resources to accommodate current players plus a little extra to handle unexpected spikes in player demand\. For most games, target\-based auto scaling offers a highly effective scaling solution\.

The topics in this section provide detailed help with the following tasks:
+ [Set minimum and maximum limits for capacity scaling](fleets-capacity-limits.md)
+ [Manually set capacity levels](fleets-updating-capacity.md)
+ [Use target\-based auto scaling](fleets-autoscaling-target.md)
+ [Manage rule\-based auto scaling \(advanced feature\)](fleets-autoscaling-rule.md)
+ [Temporarily disable auto scaling](fleets-updating-capacity.md#fleets-updating-capacity-disable)

You can do most fleet scaling activities using the GameLift console\. You can also use an AWS SDK or the AWS Command Line Interface \(AWS CLI\) with the [GameLift service API](https://docs.aws.amazon.com/gamelift/latest/apireference/Welcome.html)\.

## To manage fleet capacity in the console<a name="fleet-manage-capacity-howto"></a>

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. In the navigation pane, choose **Hosting**, **Fleets**\.

1. On the **Fleets** page, choose the name of an active fleet to open the fleet's detail page\.

1. Choose the **Scaling** tab\. On this tab, you can:
   + View historical scaling metrics for the entire fleet\.
   + View and update capacity settings for each fleet location, including scaling limits and current capacity settings\.
   + Update target\-based auto scaling, view rule\-based auto scaling policies applied to the entire fleet, and suspend auto scaling activity for each location\.

**Topics**
+ [To manage fleet capacity in the console](#fleet-manage-capacity-howto)
+ [Set GameLift capacity limits](fleets-capacity-limits.md)
+ [Manually set capacity for a GameLift fleet](fleets-updating-capacity.md)
+ [Auto scale fleet capacity with GameLift](fleets-autoscaling.md)