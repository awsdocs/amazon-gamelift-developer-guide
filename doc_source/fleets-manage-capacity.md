# Scaling GameLift hosting capacity<a name="fleets-manage-capacity"></a>

Hosting capacity, which is measured in instances, represents the number of game sessions that can be hosted concurrently, and by extension the number of concurrent players that those game sessions can accommodate\. One of the most challenging tasks with game hosting is to scale capacity so that it meets player demand without wasting money on resources that aren't needed\. Learn more about [how capacity scaling works](gamelift-howitworks.md#gamelift-howitworks-capacity) in GameLift\. 

You have full control to manage the hosting capacity of your fleets\. Capacity is adjusted at the fleet location level\. All fleets have at least one location: the fleet's home Region\. When viewing or scaling capacity, the information is listed by location, including the fleet's home Region and any additional remote locations\.

You can manually set the number of instances to maintain, or you can set up auto\-scaling to dynamically adjust capacity as player demand changes\. We recommend that you start by turning on the Target Tracking auto\-scaling option\. The idea behind target tracking is to maintain just enough hosting resources to accommodate current players plus a little extra to handle unexpected spikes in player demand\. For most games, target tracking offers a highly effective scaling solution\.

The topics in this section provide detailed help with the following tasks:
+ [Set minimum and maximum limits for capacity scaling](fleets-capacity-limits.md)
+ [Manually set capacity levels](fleets-updating-capacity.md)
+ [Turn on auto\-scaling with Target Tracking](fleets-autoscaling-target.md)
+ [Manage rule\-based auto\-scaling \(advanced feature\)](fleets-autoscaling-rule.md)
+ [Temporarily disable auto\-scaling](fleets-updating-capacity.md#fleets-updating-capacity-disable)

Most fleet scaling activities can be done using the GameLift console\. You can also use the AWS SDK or AWS CLI with the [GameLift service API](https://docs.aws.amazon.com/gamelift/latest/apireference/)\. 

## To manage fleet capacity in the console<a name="fleet-manage-capacity-howto"></a>

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Fleets** page, click the name of an active fleet to open the fleet's detail page\. \(You can also access a fleet's detail page via the **Dashboard**\.\) 

1. Open the **Scaling** tab\. In this tab, you can: 
   + View historical scaling metrics for the entire fleet or by individual fleet location\.
   + View and update capacity settings for each fleet location, including scaling limits and current capacity settings\.
   + Update target tracking auto\-scaling, view rule\-based auto\-scaling policies that are applied to the entire fleet, and enable/disable auto\-scaling activity for each location\.

**Topics**
+ [To manage fleet capacity in the console](#fleet-manage-capacity-howto)
+ [Set GameLift capacity limits](fleets-capacity-limits.md)
+ [Manually set capacity for a GameLift fleet](fleets-updating-capacity.md)
+ [Auto\-scale fleet capacity with GameLift](fleets-autoscaling.md)