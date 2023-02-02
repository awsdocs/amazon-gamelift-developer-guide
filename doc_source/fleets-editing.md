# Manage your GameLift fleets<a name="fleets-editing"></a>

Use the Amazon GameLift console or the AWS CLI to update your fleet settings, change remote locations, or delete a fleet\.

## Update a fleet configuration<a name="fleets-update"></a>

You can update mutable fleet attributes, port settings, and runtime configurations using the GameLift console or the AWS CLI\. To change scaling limits, see [Auto\-scale fleet capacity with GameLift](fleets-autoscaling.md)\.

------
#### [ GameLift console ]

1. In the [GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Fleets**\.

1. Choose the fleet you want to update\. A fleet must be in `ACTIVE` status before you can edit it\.

1. On the Fleet detail page, in any of the following sections, choose **Edit**\.
   + **Fleet settings**
     + Change the fleet attributes such as **Name** and **Description**\. 
     + Add or remove **Metric groups**, that Amazon CloudWatch uses to track aggregated GameLift metrics for multiple fleets\.
     + Update **Resource creation limit** settings\. 
     + Turn game session protection on or off\.
   + **Runtime configuration** – You can change any of the following settings of your runtime configurations and add or remove runtime configurations\.
     + Change the **Launch path** of your game server\.
     + Add, remove, or change optional **Launch parameters**\.
     + Change the number of **Concurrent processes** that your game servers run\.
   + **Game session activation** – Change how you want server processes to run and host game sessions by updating **Max concurrent game session activations** and **New activation timeout**\.
   + **EC2 port settings** – Update the IP addresses and port ranges that allow inbound access to the fleet\.

1. Choose **Confirm** to save changes\.

------
#### [ AWS CLI ]

 

Use the following AWS CLI commands to update a fleet:
+ [update\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-attributes.html)
+ [update\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html)
+ [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)

------

## Update fleet locations<a name="fleets-update-locations"></a>

You can add or remove a fleet's remote locations using the GameLift console or the AWS CLI\. You can't change a fleet's home Region\.

------
#### [ GameLift console ]

1. In the [GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Fleets**\.

1. Choose the fleet you want to update\. A fleet must be in `ACTIVE` status before you can edit it\.

1. On the Fleet detail page, choose the **Locations** tab to view the fleet's locations\. 

1. To add new remote locations, choose **Add** and select the locations you want to deploy instances to\. This list doesn't include instances where the fleet's instance type isn't available\.

1. With new locations selected, choose **Add**\. GameLift adds the new locations to the list, with status set to `NEW`\. GameLift then begins provisioning an instance in each added location and preparing it to host game sessions\.

1. To remove existing remote locations from the fleet, use the check boxes to select one or more listed locations\. 

1. With one or more fleets selected, choose **Remove**\. The removed locations remain in the list, with status set to `DELETING`\. GameLift then begins the process of terminating activity in the removed location\. If there are active instances that are hosting game sessions, GameLift uses the game server termination process to gracefully end game sessions, terminate game servers, and shut down instances\.

------
#### [ AWS CLI ]

 

Use the following AWS CLI commands to update fleet locations:
+ [create\-fleet\-locations](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet-locations.html)
+ [delete\-fleet\-locations](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-fleet-locations.html)

------

## Delete a fleet<a name="fleets-deleting"></a>

You can delete a fleet when you no longer need it\. Deleting a fleet permanently removes all data associated with game sessions and player sessions, and collected metric data\. As an alternative, you can retain the fleet, disable auto\-scaling, and manually scale the fleet to 0 instances\.

**Note**  
If the fleet has a VPC peering connection, first request authorization by calling [CreateVpcPeeringAuthorization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html)\. GameLift deletes the VPC peering connection during fleet deletion\. 

You can use either the Amazon GameLift console or the AWS CLI tool to delete a fleet\. 

------
#### [ GameLift console ]

1. In the [GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Fleets**\.

1. Choose the fleet you want to delete\. You can only delete fleets in `ACTIVE` or `ERROR` status\.

1. Choose **Delete**\.

1. In the **Delete fleet** dialog box, confirm the deletion by entering **delete**\.

1. Choose **Delete**\.

------
#### [ AWS CLI ]

 

Use the following AWS CLI command to delete a fleet:
+ [delete\-fleet](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-fleet.html)

------