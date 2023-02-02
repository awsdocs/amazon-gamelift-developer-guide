# Add an alias to a GameLift fleet<a name="aliases-creating"></a>

A Amazon GameLift *alias* is used to abstract a fleet designation\. Fleet designations tell GameLift where to search for available resources when creating new game sessions for players\. Use aliases instead of specific fleet IDs to seamlessly switch player traffic from one fleet to another by changing the alias's target location\. 

There are two types of routing strategies for aliases:
+ **Simple** – Routes player traffic to a specified fleet ID\. You can update the fleet ID for an alias at any time\.
+ **Terminal** – Passes a message back to the client\. For example, you can direct players who are using an out\-of\-date client to a location where they can get an upgrade\. 

Fleets have a finite lifespan, and there are several reasons to switch out fleets during the life of a game\. You can't update a fleet's game server build or change certain computing resource attributes on an existing fleet\. Instead, create new fleets with the changes and then switch players to the new fleets\. With aliases, switching fleets has minimal impact on your game and is invisible to players\.

Aliases are useful in games that don't use queues\. Switching fleets in a queue is a simple matter of creating a new fleet, adding it to the queue, and removing the old fleet, none of which is visible to players\. In contrast, game clients that don't use queues must specify which fleet to use when communicating with the GameLift service\. Without aliases, a fleet switch requires updates to your game code and possibly distribution of an updated game clients to players\.

When updating the fleet\-id an alias points to, there is a transition period of up to 2 minutes where game sessions on the alias may end up on the old fleet\.

## Create a new alias<a name="aliases-creating-new"></a>

You can create an alias using either the GameLift console, as described here, or with the AWS CLI command [create\-alias](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-alias.html)\.

1. In the [GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Aliases**\.

1. On the **Aliases** page, choose **Create alias**\. We recommend including the fleet type in your alias names\. This makes it much easier to identify the fleet type when viewing a list of aliases\.

1. On the **Create alias** page, under **Alias details**, do the following:

   1. For **Name**, enter an alias name\.

   1. For description, enter a short description for identification\.

   1. Choose **Simple** or **Terminal** routing type\.

1. \(Optional\) Under **Tags**, add tags to the alias by entering **Key** and **Value** pairs\.

1. Choose **Create**\.

## Edit an alias<a name="aliases-editing"></a>

You can edit an alias using the GameLift console or with the AWS CLI command [update\-alias](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-alias.html)\.

1. In the [GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Aliases**\.

1. On the **Aliases** page, choose the alias you want to edit\.

1. On alias page choose **Edit**\.

1. On the **Edit alias** page, you can edit the following:
   + **Alias name** – Friendly name for your alias\.
   + **Description** – Short description for your alias\.
   + **Type** – Routing strategy for player traffic\. Select **Simple** to change the associated fleet or select **Terminal** to edit the termination message\.

1. Choose **Save changes**\.