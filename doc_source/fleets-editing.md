# Manage your GameLift fleets<a name="fleets-editing"></a>

Use the GameLift console or the AWS CLI to update your fleet settings or delete a fleet\. You can update the following fleet settings: 
+ General fleet attributes\. Change fleet settings including name and description, game session protection, resource creation limits, and metric group\. These attributes apply to the entire fleet and, where relevant, to instances in all fleet locations\.
+ Port settings\. Add or remove inbound permissions for game servers in the fleet\. These settings define access permissions for inbound traffic that connects to game servers on the instances in all fleet locations\.
+ Runtime configuration\. Change the instructions that determine which server processes to run concurrently on an instance\. The runtime configuration is used by the instances in all fleet locations\. Updates are routinely provided to every instance\.
+ Remote locations\. Add or remove remote locations from a fleet\. A fleet's locations determine where fleet instances are deployed to host game sessions\.

## To update a fleet configuration<a name="fleets-update"></a>

You can update mutable fleet attributes, port settings and runtime configurations using the GameLift console or the AWS CLI\. To change scaling limits, see [Auto\-scale fleet capacity with GameLift](fleets-autoscaling.md)\.

**Note**  
It is possible for an active fleet to be deployed with a build that is currently in **Deleted** or **Error** status\. This does not affect the fleet's status or ability to host game sessions\. 

------
#### [ GameLift Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Choose **Fleets** from the menu bar to view a list of fleets, and click on the name of the fleet you want to update\. A fleet must be in `ACTIVE` status before it can be edited\.

1. On the Fleet detail page, under **Actions**, choose **Edit fleet**\.

1. On the **Edit fleet** page, you can make the following updates \(see [Deploy a GameLift fleet with a custom game build](fleets-creating.md) for more detailed field descriptions\):
   + Change the fleet attributes such as **Name** and **Description**\. 
   + Add or remove **Metric groups**, which are used in Amazon CloudWatch to track aggregated GameLift metrics for multiple fleets\.
   + Change how you want server processes to run and host game sessions by updating the **Server process allocation** \(runtime configuration\) and game session activation settings\. 
   + Update the ** EC2 port settings** used to connect to server processes on this fleet\. 
   + Update **resource creation limit** settings\. 
   +  Turn game session protection on or off\.

1. Click **Submit** to save your changes\.

------
#### [ AWS CLI ]

 [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

Use the following AWS CLI commands to update a fleet:
+ [update\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-attributes.html)
+ [update\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html)
+ [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)

------

## To update fleet locations<a name="fleets-update-locations"></a>

You can add or remove a fleet's remote locations using the GameLift console or the AWS CLI\. A fleet's home Region, which is where the fleet was created and resides, cannot be changes\.

------
#### [ GameLift Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Choose **Fleets** from the menu bar to view a list of fleets, and click on the name of the fleet you want to update\. A fleet must be in ACTIVE status before it can be edited\.

1. On the Fleet detail page, open the **Locations** tab to view the fleet's locations\. This list includes the fleet's home Region \(labelled\) and all remote locations\. This view displays each location's current status, and number of active instances, game servers, and game sessions\.

1. To add new remote locations, click **Add locations** and select the locations you want to deploy instances to\. This list does not include instances where the fleet's instance type is not available\. You can add some or all available locations to the fleet\. 

1. With new locations selected, click **Save**\. The new locations are added to the list, with status set to `NEW`\. GameLift immediately begins provisioning an instance in each added location and preparing it to host game sessions\. As this process moves forward, the location status changes until it is `ACTIVE`\.

1. To remove existing remote locations from the fleet, use the check boxes to select one or more listed locations\. 

1. With one or more fleets selected, click **Actions > Remove locations**\. The removed locations remain in the list, with status set to `DELETING`\. GameLift immediately begins the process of terminating activity in the removed location\. If there are active instances that are hosting game sessions, GameLift used the game server termination process to gracefully end game sessions, terminate game servers, and shut down instances\.

------
#### [ AWS CLI ]

 [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

Use the following AWS CLI commands to update fleet locations:
+ [create\-fleet\-locations](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet-locations.html)
+ [delete\-fleet\-locations](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-fleet-locations.html)

------

## To delete a fleet<a name="fleets-deleting"></a>

You can delete a fleet when it is no longer needed\. Deleting a fleet permanently removes all data associated with game sessions and player sessions, as well as collected metric data\. As an alternative, you can retain the fleet, disable auto\-scaling, and manually scale the fleet to 0 instances\.

**Note**  
If the fleet being deleted has a VPC peering connection, you first need to request authorization by calling [CreateVpcPeeringAuthorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html)\. You do not need to explicitly delete the VPC peering connection—this is done as part of the delete fleet process\. 

You can use either the Amazon GameLift console or the AWS CLI tool to delete a fleet\. 

------
#### [ GameLift Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Choose **Fleets** from the menu bar to view a list of fleets, and click on the name of the fleet you want to delete\. Only fleets in `ACTIVE` or `ERROR` status can be deleted\.

1. At the top of the **Fleet** detail page, under **Actions**, choose **Terminate fleet**\.

1. In the **Terminate fleet** dialog box, confirm the deletion by typing the name of the fleet\.

1. Click **Delete**\.

------
#### [ AWS CLI ]

 [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

Use the following AWS CLI command to delete a fleet:
+ [delete\-fleet](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-fleet.html)

------