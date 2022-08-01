# Set GameLift capacity limits<a name="fleets-capacity-limits"></a>

When scaling hosting capacity for a GameLift fleet location, either manually or by auto scaling, consider the location's scaling limits\. All fleet locations have a minimum and maximum limit that define the allowed range for the location's capacity\. By default, limits on fleet locations have a minimum of 0 instances and a maximum of 1 instance\. Before you can scale a fleet location, adjust the limits\.

If you're using auto scaling, the maximum limit allows GameLift to scale up a fleet location to meet player demand but prevents runaway hosting costs, such as during a DDOS attack\. Set up an [Amazon CloudWatch alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) to notify you when capacity approaches the maximum limit, so you can evaluate the situation and manually adjust as needed\. \(You can also [create a billing alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html) to monitor AWS costs\.\) The minimum limit is useful to maintain hosting availability, even when player demand is low\.

You can set capacity limits for a fleet's locations in the [GameLift console](https://console.aws.amazon.com/gamelift/) or by using the AWS Command Line Interface \(AWS CLI\)\.

## To set capacity limits<a name="fleets-capacity-limits-console"></a>

------
#### [ Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. In the navigation pane, choose **Hosting**, **Fleets**\.

1. On the **Fleets** page, choose the name of an active fleet to open the fleet's detail page\.

1. On the **Scaling** tab, under **Scaling capacity**, select a fleet location, and then choose **Edit**\.

1. In the **Edit scaling capacity** dialog box, set instance counts for **Min size**, **Desired instances**, and **Max size**\.

1. Choose **Confirm**\.

------
#### [ AWS CLI ]

1. **Check current capacity settings\.** In a command line window, use the [describe\-fleet\-location\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-location-capacity.html) command with the fleet ID and location that you want to change capacity for\. This command returns a [FleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetCapacity.html) object that includes the location's current capacity settings\. Determine whether the new instance limits can accommodate the current desired instances setting\.

   ```
   aws gamelift describe-fleet-location-capacity \
       --fleet-id <fleet identifier> \
       --location <location name>
   ```

1. **Update limit settings\.** In a command line window, use the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command with the following parameters\. You can adjust both instance limits and desired instance count with the same command\.

   ```
   --fleet-id <fleet identifier>
   --location <location name>
   --max-size <maximum capacity for scaling>
   --min-size <minimum capacity for scaling>
   --desired-instances <fleet capacity goal>
   ```

   Example:

   ```
   aws gamelift update-fleet-capacity \
       --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa \
       --location us-west-2 \
       --max-size 10 \
       --min-size 1 \
       --desired-instances 10
   ```

If your request is successful, GameLift returns the fleet ID\. If the new `max-size` or `min-size` value conflicts with the current `desired-instances` setting, GameLift returns an error\.

------