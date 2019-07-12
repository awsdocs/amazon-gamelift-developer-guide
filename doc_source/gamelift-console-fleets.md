# View Your Fleets<a name="gamelift-console-fleets"></a>

You can view information on all the fleets created to host your games on Amazon GameLift under your AWS account\. Fleets shown include only those created in the selected region\. From the **Fleets** page, you can create a new fleet or view additional detail on a selected fleet\. A fleet's [detail page]() contains usage information and metrics as well as game session and player session data; you can also edit the fleet record or terminate the fleet\.

To view the **Fleets** page, choose **Fleets** from the Amazon GameLift console's menu bar\.

The **Fleets** page displays the following summary information by default\. You can customize which information to display by using the **Settings** \(gear\) button \.
+ **Status** – The status of the fleet, which can be one of these states: **New**, **Downloading**, **Building**, and **Active**\. A fleet must be in Active status to be able to host game sessions and allow player connections\.
+ **Fleet name** – Friendly name given to the fleet\.
+ **EC2 type** – The Amazon EC2 instance type, which determines the computing capacity of fleet's instances\.
+ **OS** – Operating system on each instances in the fleet\. A fleet's OS is determined by the build deployed to it\.
+ **Active instances** – The number of EC2 instances in use for the fleet\.
+ **Maximum instances** – The current maximum number of EC2 instances allowed on the fleet\. This value is configurable \(within service limits\) from the Fleet detail page, Scaling tab\.
+ **Game sessions** – The number of active game sessions currently running in the fleet\. The data is delayed five minutes\.
+ **Player sessions** – The number of players connected to game sessions in the fleet\. The data is delayed five minutes\.
+ **Uptime** – The total length of time the fleet has been running\.
+ **Date created** – The date and time the fleet was created\.