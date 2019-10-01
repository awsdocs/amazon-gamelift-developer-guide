# Manually Set Fleet Capacity<a name="fleets-updating-capacity"></a>

When you create a new fleet, fleet capacity is automatically set to one instance\. In response, Amazon GameLift starts one new instance with game server processes as configured\. To change fleet capacity, you can either turn on auto\-scaling or you can manually set the number of instances you want for the fleet\. Learn more about [Scaling Fleet Capacity](gamelift-howitworks.md#gamelift-howitworks-capacity)\. 

Setting a fleet's capacity manually can be useful when auto\-scaling is not needed or when you need to hold capacity at an arbitrary number of instances temporarily or permanently\. When manually setting a desired capacity, keep in mind that this action will affect actual fleet capacity only if \(1\) there are no auto\-scaling policies for the fleet, or \(2\) auto\-scaling has been disabled\. If auto\-scaling is enabled, it will immediately reset the desired capacity based on its own scaling rules\. 

You can manually set fleet capacity in the Amazon GameLift console or by using the AWS CLI\. The fleet's status must be **Active**\. 

## Disabling Auto\-Scaling<a name="fleets-updating-capacity-disable"></a>

Disabling auto\-scaling allows you to turn off all auto\-scaling for a fleet and return to manual scaling\. While you can always delete a fleet's autoscaling policy, this feature lets you temporarily turn off auto\-scaling but retain the policies for future use\. For example, if you want to scale up in preparation for an upcoming major event, you can disable auto\-scaling, manually set a desired fleet capacity, and then, once the event is in progress, re\-enable auto\-scaling\. When auto\-scaling is disabled, all auto\-scaling activities for the fleet are stopped, including all currently active policies and any policies that might be created in the future\. You can enable/disable auto\-scaling in the Amazon GameLift console \(see Step 4 in "To manually set capacity"\)\.

## To Manually Set Capacity \(Console\)<a name="fleets-updating-capacity-console"></a>

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Fleets** page, click the name of an active fleet to open the fleet's detail page\. \(You can also access a fleet's detail page via the **Dashboard**\.\) 

1. Open the **Scaling** tab to view historical scaling metrics and to view or change the current settings\. Scaling settings are located below the metrics graph\.

1. Under **Auto\-Scaling Policies**, check the box "Disable all scaling policies" and commit your change by clicking the check mark button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/checkmark.png)\. This setting stops all auto\-scaling actions for the fleet\. It is a good idea to do this even if the fleet currently has no policies, as it will prevent any newly created policies from taking effect\. Once you commit this change, the table listing active scaling policies indicates that all policies are "disabled"\.

1. Under **Auto\-Scaling Policies**, for the option "Manually adjust the desired instance count to\.\.\.", specify the number of instances for the fleet\. This value tells Amazon GameLift how many instances to maintain in an active state, ready to host game sessions\. Commit the change by clicking the checkmark button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/checkmark.png)\. 

   If the new desired instances value is outside the fleet's capacity limits, you'll get an error\. In this case, you must first adjust the fleet's instance limits to allow for the new desired instance count\. Instance Limits can also be set on the **Scaling** tab\.

As soon as you commit changes to instance limits and manual scaling levels, the new values are reflected in the graph at the top of the **Scaling** tab\. Amazon GameLift immediately begins responding to the changes by deploying additional instances or shutting down unneeded ones\. As this process is completed, the number of **Active** instances change to match the newly updated desired value\. This process may take a little time\.

## To Manually Set Capacity \(AWS CLI\)<a name="fleets-updating-capacity-cli"></a>

1. **Check current capacity settings\. **In a command line window, use the [describe\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-capacity.html) command with the fleet ID of the fleet you want to change capacity for\. This command returns a [FleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetCapacity.html) object that includes the current instance count and capacity limits\. Determine whether the new instance count falls between the minimum and maximum limits\.

   ```
   aws gamelift describe-fleet-capacity --fleet-id <unique fleet identifier>
   ```

1. **Update desired capacity\.** Use the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command with the fleet ID and a new *desired\-instances* value\. If this value falls outside the current limit range, include adjust limit values in the same command\. 

   ```
   --fleet-id <unique fleet identifier>
   --desired-instances <fleet capacity as an integer>
   --max-size <maximum capacity for auto-scaling>    [Optional]
   --min-size <minimum capacity for auto-scaling>    [Optional]
   ```

   Example:

   ```
   aws gamelift update-fleet-capacity
   --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa
   --desired-instances 5
   --max-size 10
   --min-size 1
   ```

   *Copyable version:*

   ```
   aws gamelift update-fleet-capacity --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa --desired-instances 5 --max-size 10 --min-size 1
   ```

If your request is successful, the fleet ID is returned\. If the new *desired\-instances* setting is outside the minimum/maximum limits, an error is returned\.