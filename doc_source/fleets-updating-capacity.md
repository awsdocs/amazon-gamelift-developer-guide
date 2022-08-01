# Manually set capacity for a GameLift fleet<a name="fleets-updating-capacity"></a>

When you create a new fleet, GameLift automatically sets the desired instances to one instance in each fleet location\. Then, GameLift deploys one new instance in each location\. To change fleet capacity, you can add a target\-based auto scaling policy, or you can manually set the number of instances that you want for a location\. For more information, see [Scaling fleet capacity](gamelift-howitworks.md#gamelift-howitworks-capacity)\.

Setting a fleet's capacity manually can be useful when you don't need auto scaling or when you need to hold capacity at a specified level\. Manually setting capacity works only if you aren't using a target\-based auto scaling policy\. If you have a target\-based auto scaling policy, it immediately resets the desired capacity based on its own scaling rules\.

You can manually set capacity in the GameLift console or by using the AWS Command Line Interface \(AWS CLI\)\. The fleet's status must be active\.

## Suspend auto scaling<a name="fleets-updating-capacity-disable"></a>

You can suspend all auto scaling activity for each fleet location\. With auto scaling suspended, the desired number of instances in the fleet location remains the same unless manually changed\. When you suspend auto scaling for a location, it affects the fleet's current policies and any policies that you may define in the future\.

## To manually set fleet capacity<a name="fleets-updating-capacity-console"></a>

------
#### [ Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. In the navigation pane, choose **Hosting**, **Fleets**\.

1. On the **Fleets** page, choose the name of an active fleet to open the fleet's detail page\.

1. On the **Scaling** tab, under **Suspended auto\-scaling locations**, select each location that you want to suspend auto scaling for, and then choose **Suspend**\.

1. Under **Scaling capacity**, select a location that you want to set manually, and then choose **Edit**\.

1. In the **Edit scaling capacity** dialog box, set your preferred value for **Desired instances**, and then choose **Confirm**\. This tells GameLift the number of instances to maintain in an active state, ready to host game sessions\.

GameLift responds to the changes by deploying additional instances or shutting down unneeded ones\. As GameLift completes this process, the number of active instances in the location changes to match the updated desired instances value\. This process may take a little time\.

------
#### [ AWS CLI ]

1. **Check current capacity settings\.** In a command line window, use the [describe\-fleet\-location\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-location-capacity.html) command with the fleet ID and location that you want to change capacity for\. This command returns a [FleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetCapacity.html) object that includes the location's current capacity settings\. Determine whether the instance limits can accommodate the new desired instances setting\.

   ```
   aws gamelift describe-fleet-location-capacity \
       --fleet-id <fleet identifier> \
       --location <location name>
   ```

1. **Update desired capacity\.** Use the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command with the fleet ID, location, and a new value for desired instances\. If this value falls outside the current limit range, you can adjust limit values in the same command\.

   ```
   --fleet-id <fleet identifier>
   --location <location name>
   --desired-instances <fleet capacity as an integer>
   --max-size <maximum capacity>    [Optional]
   --min-size <minimum capacity>    [Optional]
   ```

   Example:

   ```
   aws gamelift update-fleet-capacity \
       --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa \
       --location us-west-2 \
       --desired-instances 5 \
       --max-size 10 \
       --min-size 1
   ```

If your request is successful, GameLift returns the fleet ID\. If the new desired instances setting is outside the minimum and maximum limits, GameLift returns an error\.

------