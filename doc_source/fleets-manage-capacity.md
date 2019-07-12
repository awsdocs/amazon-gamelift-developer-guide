# Scaling Amazon GameLift Fleet Capacity<a name="fleets-manage-capacity"></a>

Fleet capacity, measured in instances, determines the number of game sessions \(and players\) the fleet can host\. One of the most challenging tasks with game hosting is to strike a balance between maintaining enough capacity to accommodate every new player and not wasting money on unnecessary resources\. Learn more about [how capacity scaling works](gamelift-howitworks.md#gamelift-howitworks-capacity) in Amazon GameLift\. 

You have full control over scaling a fleet\. You can set capacity to a specific number of instances, or you can take advantage of auto\-scaling to adjust capacity based on actual player demand\. We recommended that you start by turning on the auto\-scaling option Target Tracking\. Target tracking is an effective and easy to use tool for that helps you maintain just enough hosting resource to accommodate current player demand and handle sudden spikes\. For the vast majority games, target tracking will be the only solution you need\. 

The topics in this section provide detailed help with the following tasks:
+ [Set minimum and maximum limits for capacity scaling](fleets-capacity-limits.md)
+ [Manually set capacity levels](fleets-updating-capacity.md)
+ [Turn on auto\-scaling with Target Tracking](fleets-autoscaling-target.md)
+ [Manage rule\-based auto\-scaling \(advanced feature\)](fleets-autoscaling-rule.md)
+ [Temporarily disable auto\-scaling](fleets-updating-capacity.md#fleets-updating-capacity-disable)

Most fleet scaling activities can be done using the Amazon GameLift console\. You can also use the AWS SDK or AWS CLI with the [Amazon GameLift Service API](https://docs.aws.amazon.com/gamelift/latest/apireference/) for all fleet scaling\. 

**To access fleet capacity settings in the console:**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Fleets** page, click the name of an active fleet to open the fleet's detail page\. \(You can also access a fleet's detail page via the **Dashboard**\.\) 

1. Open the **Scaling** tab to view historical scaling metrics and to view or change the current settings\. Settings are located below the metrics graph\. In this section, you can view or update scaling limits, manually set fleet capacity, enable or disable auto\-scaling, turn on target\-based auto\-scaling, and view all active auto\-scaling policies\. 

**Topics**
+ [Set Fleet Capacity Limits](fleets-capacity-limits.md)
+ [Manually Set Fleet Capacity](fleets-updating-capacity.md)
+ [Auto\-Scale Fleet Capacity](fleets-autoscaling.md)