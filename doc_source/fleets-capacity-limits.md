# Set GameLift capacity limits<a name="fleets-capacity-limits"></a>

When scaling hosting capacity for a fleet location, either manually or by auto\-scaling, you must consider the location's scaling limits\. All fleet locations have a minimum and maximum limit that define the allowed range for the location's capacity\. By default, limits on fleet locations are set to a minimum of 0 instances and a maximum of 1 instance\. Before you can scale a fleet location, you'll need to adjust the limits\. 

If you're using auto\-scaling, the maximum limit allows GameLift scale up a fleet location to meet player demand but prevents runaway hosting costs, such as might occur during a DDOS attack\. Set up CloudWatch to alarm when capacity approaches the maximum limit, so you can evaluate the situation and manually adjust as needed\. \(You can also [set up a billing alert](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/monitor-charges.html) to monitor AWS costs\.\) The minimum limit is useful to maintain some hosting availability at all times, even when player demand is low\. 

You can set capacity limits for a fleet's locations in the [GameLift console](https://console.aws.amazon.com/gamelift/) or by using the AWS CLI\. The location's status must be **Active**\.

## To set capacity limits<a name="fleets-capacity-limits-console"></a>

------
#### [ Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Fleets** page, click the name of an active fleet to open the fleet's detail page\. \(You can also access a fleet's detail page via the **Dashboard**\.\) 

1. Open the **Scaling** tab\. Capacity scaling settings are located in the **Scaling Limits** section\.

1. For each fleet location, set minimum and maximum instance counts\. After making a change, you must commit your changes by clicking the check mark button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/checkmark.png)\. 

   If the location's current desired instance count is outside the new limit range, you'll get an error\. In this case, you must first adjust the desired instance count so that it falls inside the new limit range\. If the fleet uses auto\-scaling, you'll need to disable auto\-scaling for the location, manually adjust the desired instance count, and set the new limit range, then re\-enable auto\-scaling\.

The new limits are immediately reflected in the **Scaling History** graph\.

------
#### [ AWS CLI ]

1. **Check current capacity settings\.** In a command line window, use the [describe\-fleet\-location\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-location-capacity.html) command with the fleet ID and location that you want to change capacity for\. This command returns a [FleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetCapacity.html) object that includes the location's current capacity settings\. Determine whether the new instance limits will accommodate the current desired instances setting\.

   ```
   AWS gamelift describe-fleet-location-capacity --fleet-id <fleet identifier> --location <location name>
   ```

1. **Update limit settings\.** In a command line window, use the [ update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command with the following parameters\. You can adjust both instance limits and desired instance count with the same command\.

   ```
   --fleet-id <fleet identifier>
   --location <location name>
   --max-size <maximum capacity for scaling>
   --min-size <minimum capacity for scaling>
   --desired-instances <fleet capacity goal>    [Optional]
   ```

   Example:

   ```
   AWS gamelift update-fleet-capacity
   --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa
   --location us-west-2
   --max-size 10
   --min-size 1
   --desired-instances 10
   ```

   *Copyable version:*

   ```
   AWS gamelift update-fleet-capacity --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa --location us-west-2 --max-size 10 --min-size 1 --desired-instances 10             
   ```

If your request is successful, the fleet ID is returned\. If the new *max\-size* or *min\-size* value conflicts with the current *desired\-instances* setting, an error is returned\.

------