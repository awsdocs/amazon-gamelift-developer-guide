# Set Fleet Capacity Limits<a name="fleets-capacity-limits"></a>

Fleet size is determined by the number of instances it contains\. Each fleet has a defined minimum and maximum limit, which you can set as needed for your game\. All requests to change fleet capacity \(either by auto\-scaling or manual adjustment\) must fall within the current limits\. By default, limits on new fleets are set to a minimum of 0 instances and a maximum of 1 instance\. Before scaling up a fleet, you'll need to adjust the fleet's limits\. 

If you're auto\-scaling a fleet, the maximum limit lets Amazon GameLift scale up the fleet as needed to accommodate player demand while also preventing runaway hosting costs, such as might occur during a DDOS attack\. Set up CloudWatch to alarm when capacity approaches the maximum limit, so you can evaluate the situation and manually adjust as needed\. \(You can also [set up a billing alert](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/monitor-charges.html) to monitor AWS costs\.\) The minimum limit is useful when you want to ensure that you always have some hosting availability at all times\. 

Limits also apply to manually scaled fleets\. Before you can adjust fleet capacity to a value outside the limit range, you must change the limits\. Since fleet capacity has such a significant effect on game availability and player experience, the limits feature provides an additional layer of control over capacity\. 

You can set a fleet's capacity limits in the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/) or by using the AWS CLI\. The fleet's status must be **Active**\. 

## To Set Capacity Limits \(Console\)<a name="fleets-capacity-limits-console"></a>

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Fleets** page, click the name of an active fleet to open the fleet's detail page\. \(You can also access a fleet's detail page via the **Dashboard**\.\) 

1. Open the **Scaling** tab to view historical scaling metrics and to view or change the current settings\. Scaling settings are located below the metrics graph\.

1. Under **Instance Limits**, set minimum and maximum instance counts\. For each control, commit your changes by clicking the checkmark button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/checkmark.png)\. 

   If the fleet's current desired instance count is outside the new limit range, you'll get an error\. In this case, you must first adjust the fleet's desired instance count so that it falls inside the new limit range\. This can be done on the **Scaling** tab\. If the fleet uses auto\-scaling, you'll need to disable auto\-scaling, manually adjust the desired instance count and set the new limit range, and then re\-enable auto\-scaling\.

The new limits are immediately reflected in the graph at the top of the **Scaling** tab\.

## To Set Capacity Limits \(AWS CLI\)<a name="fleets-capacity-limits-cli"></a>

1. **Check current capacity settings\. **In a command line window, use the [describe\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-capacity.html) command with the fleet ID of the fleet you want to change capacity for\. This command returns a [FleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetCapacity.html) object that includes the current instance count and capacity limits\. Determine whether the new instance limits will accommodate the current desired instances setting\.

   ```
   aws gamelift describe-fleet-capacity --fleet-id <unique fleet identifier>
   ```

1. **Update limit settings\.** In a command line window, use the [ update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command with the following parameters\. You can adjust both instance limits and desired instance count with the same command\.

   ```
   --fleet-id <unique fleet identifier>
   --max-size <maximum capacity for auto-scaling>
   --min-size <minimum capacity for auto-scaling>
   --desired-instances <fleet capacity as an integer>    [Optional]
   ```

   Example:

   ```
   aws gamelift update-fleet-capacity
   --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa
   --max-size 10
   --min-size 1
   --desired-instances 10
   ```

   *Copyable version:*

   ```
   aws gamelift update-fleet-capacity --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa --max-size 10 --min-size 1 --desired-instances 10 
   ```

If your request is successful, the fleet ID is returned\. If the new *max\-size* or *min\-size* value conflicts with the current *desired\-instances* setting, an error is returned\.