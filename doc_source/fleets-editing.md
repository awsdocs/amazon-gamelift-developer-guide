# Manage Fleet Records<a name="fleets-editing"></a>

Use the Amazon GameLift console or the AWS CLI to manage your existing fleets, including updating the fleet's attributes, port settings, and runtime configuration\. You can also delete a fleet\.

## Update a Fleet<a name="fleets-update"></a>

Use the **Edit fleet** page in the Amazon GameLift console to change a fleet's configuration\. All fleet properties can be changed except for the build ID and the instance type\. To change scaling settings, see [Auto\-Scale Fleet Capacity](fleets-autoscaling.md)\.

**Note**  
An active fleet may be deployed with a build that has been deleted or is in an error state\. This does not affect the fleet's status or ability to host game sessions\. In this situation, you may see a Build status of **Deleted** or **Error** \(if an error occurred while retrieving the build info\)\. 

------
#### [ GameLift Console ]

**To update a fleet configuration**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Choose **Fleets** from the menu bar to view a list of fleets, and click on the name of the fleet you want to update\. A fleet must be in ACTIVE status before it can be edited\.

1. On the Fleet detail page, under **Actions**, choose **Edit fleet**\.

1. On the **Edit fleet** page, you can make the following updates \(see [Deploy a GameLift Fleet for a Custom Game Build](fleets-creating.md) for more detailed field descriptions\):
   + Change the fleet attributes such as **Name** and **Description**\. 
   + Add or remove **Metric groups**, which are are used in Amazon CloudWatch to track aggregated Amazon GameLift metrics for multiple fleets\.
   + Change how you want server processes to run and host game sessions by updating the **Server process allocation** \(runtime configuration\) and game session activation settings\. 
   + Update the ** EC2 port settings** used to connect to server processes on this fleet\. 
   + Update **resource creation limit** settings\. 
   +  Turn game session protection on or off\.

1. Click **Submit** to save your changes\.

------
#### [ AWS CLI ]

Use the following AWS CLI commands to update a fleet:
+ [update\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-attributes.html)
+ [update\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html)
+ [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)

------

## Delete a Fleet<a name="fleets-deleting"></a>

You can delete a fleet when it is no longer needed\. Deleting a fleet permanently removes all data associated with game sessions and player sessions, as well as collected metric data\. As an alternative, you can retain the fleet, disable auto\-scaling, and manually scale the fleet to 0 instances\.

**Note**  
If the fleet being deleted has a VPC peering connection, you first need to request authorization by calling [CreateVpcPeeringAuthorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html)\. You do not need to explicitly delete the VPC peering connection\-\-this is done as part of the delete fleet process\. 

You can use either the Amazon GameLift console or the AWS CLI tool to delete a fleet\. 

------
#### [ GameLift Console ]

**To delete a fleet**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Choose **Fleets** from the menu bar to view a list of fleets, and click on the name of the fleet you want to delete\. Only fleets in ACTIVE or ERROR status can be deleted\.

1. On the **Fleet** detail page for your selected fleet, verify that the fleet has zero active instances\. If the fleet still has instances, go to the **Scaling** tab and do the following: 
   + Check the box **Disable all scaling policies for the fleet**\. This action stops all auto\-scaling, which would counteract your manual scaling settings\.
   + Manually adjust the desired instance count to "0"\.

   It may take several minutes for the fleet to scale down\. If any instances have active game sessions with game session protection, you'll need to either wait for the game sessions to end, or stop protection for the active game sessions \(this can't be done in the Console, see [UpdateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSession.html)\)\.

1. Once the fleet is scaled down to zero active instances, you can delete the fleet\. At the top of the **Fleet** detail page, under **Actions**, choose **Terminate fleet**\.

1. In the **Terminate fleet** dialog box, confirm the deletion by typing the name of the fleet\.

1. Click **Delete**\.

------
#### [ AWS CLI ]

**To delete a fleet**

 [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

1. In a command line window, call [describe\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-capacity.html) and verify that the fleet to be deleted has been scaled down to zero active instances\. If the fleet still has active instances:

   1. call [stop\-fleet\-actions](https://docs.aws.amazon.com/cli/latest/reference/gamelift/stop-fleet-actions.html) to disable auto\-scaling\. 

   1. Call [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) and set the parameter desired\-instances to "0"\. 

   1. Wait for the fleet to scale down to zero active instances\. This may take several minutes\. If any instances have active game sessions with game session protection, you'll need to either wait for the game sessions to end, or stop protection for the active game sessions \(see [update\-game\-session](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-game-session.html)\)\.

1. Once the fleet is scaled down, call [delete\-fleet](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-fleet.html) to delete the fleet\.

------