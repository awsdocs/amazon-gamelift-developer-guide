# Add an Alias to a Amazon GameLift Fleet<a name="aliases-creating"></a>

An Amazon GameLift *alias* is used to abstract a fleet designation\. Fleet designations tell Amazon GameLift where to search for available resources when creating new game sessions for players\. By using aliases instead of specific fleet IDs, you can more easily and seamlessly switch player traffic from one fleet to another by changing the alias's target location\. 

There are two types of routing strategies for aliases:
+ **Simple** – A simple alias routes player traffic to a specified fleet ID\. You can update the fleet ID for an alias at any time\.
+ **Terminal** – A terminal alias does not resolve to a fleet\. Instead, it passes a message back to the client\. For example, you may want to direct players who are using an out\-of\-date client to a location where they can get an upgrade\. 

Fleets have a finite lifespan, and there are several reasons why you'll need to switch out fleets during the life of a game\. Specifically, you can't update a fleet's game server build or change certain computing resource attributes \(instance types, spot/on\-demand usage\) on an existing fleet\. Instead, you need to create new fleets with the changes and then switch players to the new fleets\. With aliases, switching fleets has minimal impact on your game and is invisible to players\.

Aliases are primarily useful in games that do not use queues\. Switching fleets in a queue is a simple matter of creating a new fleet, adding it to the queue, and removing the old fleet, none of which is visible to players\. In contrast, game clients that don't use queues must specify which fleet to use when communicating with the Amazon GameLift service\. Without aliases, a fleet switch requires updates to your game code and possibly the need to distribute updated game clients to players\. With aliases, you can avoid both\. 

## Create a New Alias<a name="aliases-creating-new"></a>

You can create a new alias to resolve to a fleet\.

**To create a new alias**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Choose **Aliases** from the menu bar\.

1. On the **Aliases** page, click **Create alias**\.

1. On the **Create alias** page, in the **Alias details** section, do the following:
   + **Alias name** – Type a friendly name so you can easily identify the alias in the catalog\.
   + **Description** – \(Optional\) Type a short description for your alias to add further identification\.

1. In the **Routing options** section, for **Type**, choose **Simple** or **Terminal**:
   + If you choose **Simple**, select an available fleet to associate with your alias\. A simple alias routes player traffic to the associated fleet\.
   + If you select **Terminal**, type a message that will be displayed to players\. A terminal alias does not resolve to a fleet but only passes your message to the client\.

1. Click **Configure alias**\.

## Edit an Alias<a name="aliases-editing"></a>

Use the **Edit alias** page in the Amazon GameLift console to update the information for your alias\.

**To edit an alias**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Choose **Aliases** from the menu bar\.

1. On the **Aliases** page, click the name of the alias you want to edit\.

1. On the selected alias page, for **Actions**, choose **Edit alias**\.

1. On the **Edit alias** page, you can edit the following:
   + **Alias name** – Friendly name for your alias\.
   + **Description** – Short description for your alias\.
   + **Type** – Routing strategy for player traffic\. Select **Simple** to change the associated fleet or select **Terminal** to edit the termination message\.

1. Click **Submit**\.