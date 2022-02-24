# Manually set capacity for a GameLift fleet<a name="fleets-updating-capacity"></a>

When you create a new fleet, desired capacity is automatically set to one instance in each fleet location\. In response, GameLift deploys one new instance in each location\. To change fleet capacity, you can either turn on auto\-scaling or you can manually set the number of instances you want for a location\. Learn more about [Scaling fleet capacity](gamelift-howitworks.md#gamelift-howitworks-capacity)\. 

Setting a fleet's capacity manually can be useful when auto\-scaling is not needed or when you need to hold capacity at an arbitrary level\. When setting a desired capacity manually, keep in mind that this action will affect actual fleet capacity only as long as there are no auto\-scaling policies in force for the fleet location\. If auto\-scaling is enabled, it will immediately reset the desired capacity based on its own scaling rules\. 

You can manually set capacity in the GameLift console or by using the AWS CLI\. The fleet's status must be **Active**\. 

## Disabling auto\-scaling<a name="fleets-updating-capacity-disable"></a>

When a fleet is configured with auto\-scaling policies, you can opt to turn off all auto\-scaling activity for each fleet location\. With auto\-scaling turned off, the desired number of instances in the fleet location will remain the same unless manually changed\. When auto\-scaling is turned off for a location, it affects the fleet's current policies and any policies that might be defined in the future\. 

## To manually set fleet capacity<a name="fleets-updating-capacity-console"></a>

------
#### [ Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Fleets** page, click the name of an active fleet to open the fleet's detail page\. \(You can also access a fleet's detail page via the **Dashboard**\.\) 

1. Open the **Scaling** tab and scroll down to the **Auto\-Scaling Policies** section\. If there are auto\-scaling policies defined for the fleet, this section lists each fleet location with a **Disable all scaling policies** check box\. Disable auto\-scaling for the location that you want to set manually\.

1. In the **Scaling Limits** section, set the Desired instances field for the location you want to set manually\. This value tells GameLift how many instances to maintain in an active state, ready to host game sessions\. Commit the change by clicking the checkmark button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/checkmark.png)\. If the new desired instances value falls outside of the location's minimum and maximum limits, an error is generated\.

As soon as you commit changes to instance limits and manual scaling levels, the new values are reflected in the graph at the top of the **Scaling** tab\. GameLift immediately begins responding to the changes by deploying additional instances or shutting down unneeded ones\. As this process is completed, the number of **Active** instances in the location changes to match the updated desired instances value\. This process may take a little time\.

------
#### [ AWS CLI ]

1. **Check current capacity settings\.** In a command line window, use the [describe\-fleet\-location\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-location-capacity.html) command with the fleet ID and location that you want to change capacity for\. This command returns a [FleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetCapacity.html) object that includes the location's current capacity settings\. Determine whether the instance limits will accommodate the new desired instances setting\.

   ```
   AWS gamelift describe-fleet-location-capacity --fleet-id <fleet identifier> --location <location name>
   ```

1. **Update desired capacity\.** Use the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command with the fleet ID, location, and a new *desired\-instances* value\. If this value falls outside the current limit range, you can adjust limit values in the same command\. 

   ```
   --fleet-id <fleet identifier>
   --location <location name>
   --desired-instances <fleet capacity as an integer>
   --max-size <maximum capacity>    [Optional]
   --min-size <minimum capacity>    [Optional]
   ```

   Example:

   ```
   AWS gamelift update-fleet-capacity
   --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa
   --location us-west-2
   --desired-instances 5
   --max-size 10
   --min-size 1
   ```

   *Copyable version:*

   ```
   AWS gamelift update-fleet-capacity --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa --location us-west-2 --desired-instances 5 --max-size 10 --min-size 1
   ```

If your request is successful, the fleet ID is returned\. If the new *desired\-instances* setting is outside the minimum/maximum limits, an error is returned\.

------