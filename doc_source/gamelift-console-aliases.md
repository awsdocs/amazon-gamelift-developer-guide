# View Your Aliases<a name="gamelift-console-aliases"></a>

You can view information on all of the fleet aliases you have created and take actions on them on the Aliases page\. Aliases shown include only those created for the selected region\.

## Alias Catalog<a name="gamelift-console-aliases-catalog"></a>

All created aliases are shown on the Aliases catalog page\. To view the Aliases page, choose **Aliases** from the Amazon GameLift console's menu bar\. 

The Aliases page provides summary information on all builds, including type\. From this page you can: 
+ Create a new alias\. click **Create alias**\.
+ Filter and sort the aliases list\. Use the controls at the top of the table\.
+ View alias details\. Click an alias name to open the alias detail page\.

## Alias Detail<a name="gamelift-console-aliases-detail"></a>

Access an alias's detail page from either the console dashboard or the Aliases catalog page by clicking the alias name\. The Alias detail page displays a summary of information on the alias\. 

From this page you can: 
+ Edit an alias, including changing the name, description, and the fleet ID the alias is associated with\. Click **Actions: Edit alias**\.
+ View information on the fleet the alias is currently associated with\. This includes the fleet's status and current utilization \(active game sessions and players\)\. 
+ Delete an alias\. Click **Actions: Delete alias**\.

Alias detail information includes: 
+ **Type** – The routing option for the alias, which can be one of these:
  + **Simple** – A simple alias routes a player to games on an associated fleet\. You can update the alias to point to a different fleet at any time\.
  + **Terminal** – A terminal alias does not point to a fleet\. Instead it passes a message back to the client\. This alias type is useful for gracefully notifying players when a set of game servers is no longer available\. For example, a terminal alias might inform players that their game clients are out of date and provide upgrade options\. 
+ **Alias ID** – The unique number used to identify the alias\.
+ **Description** – The description of the alias\.
+ **Date created** – The date and time the alias was created\.