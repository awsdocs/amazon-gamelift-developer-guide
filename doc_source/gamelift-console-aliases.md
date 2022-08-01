# View your aliases<a name="gamelift-console-aliases"></a>

The **Alias** page displays information about the fleet aliases you created in your current Region\. To view the aliases page, choose **Aliases** in the navigation pane\. 

You can do the following on the aliases page:
+ Create a new alias\. Choose **Create alias**\.
+ Filter and sort the aliases table\. Use the controls at the top of the table\.
+ View alias details\. Choose an alias name to open the alias detail page\.
+ Delete an alias\. Choose an alias and then choose **Delete**\.

## Alias details<a name="gamelift-console-aliases-detail"></a>

The alias details page displays information about the alias\. 

From this page you can: 
+ Edit an alias\. Choose **Edit**\.
+ View the fleets you associated with the alias\.
+ Delete an alias\. Choose **Delete**\.

Alias detail information includes: 
+ **ID** – The unique number used to identify the alias\.
+ **Description** – The description of the alias\.
+ **ARN** – The Amazon Resource Name of the alias\.
+ **Creation** – The date and time the alias was created\.
+ **Last updated** – The date and time that the alias was last updated\.
+ **Routing type** – The routing type for the alias, which can be one of these:
  + **Simple** – Routes player traffic to a specified fleet ID\. You can update the fleet ID for an alias at any time\.
  + **Terminal** – Passes a message back to the client\. For example, you can direct players who are using an out\-of\-date client to a location where they can get an upgrade\.
+ **Tags** – Key and value pairs used to identify the alias\.