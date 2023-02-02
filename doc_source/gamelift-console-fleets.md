# View your fleets<a name="gamelift-console-fleets"></a>

You can view information on all the fleets created to host your games on Amazon GameLift under your AWS account\. The list shows fleets created your current Region\. From the **Fleets** page, you can create a new fleet or view additional detail on a fleet\. A fleet's [detail page](gamelift-console-fleets-metrics.md) contains usage information, metrics, game session data, and player session data\. You can also edit a fleet record or delete a fleet\.

To view the **Fleets** page, choose **Fleets** from the navigation pane\.\.

The **Fleets** page displays the following summary information by default\. You can customize the information shown by choosing the **Settings** \(gear\) button\.
+ **Name** – Friendly name given to the fleet\.
+ **Status** – The status of the fleet, which can be one of these states: **New**, **Downloading**, **Building**, and **Active**\.
+ **Creation time** – The date and time the fleet was created\.
+ **Compute type** – The type of compute used to host your games\. A fleet can be a **Managed EC2** fleet or a **Anywhere** fleet\.
+ **Instance type** – The Amazon EC2 instance type, which determines the computing capacity of fleet's instances\.
+ **Active instances** – The number of EC2 instances in use for the fleet\.
+ **Desired instances** – The number of EC2 instances to keep active\.
+ **Game sessions** – The number of active game sessions running in the fleet\. The data is delayed by five minutes\.