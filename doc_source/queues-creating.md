# Create a game session queue<a name="queues-creating"></a>

Queues are used to place new game sessions with the best available hosting resources across multiple fleets and regions\. To learn more about building queues for your game, see [Design a game session queue](queues-design.md)\.

In a game client, new game sessions are started with queues by using placement requests\. Learn more about game session placement in [Create game sessions](gamelift-sdk-client-api.md#gamelift-sdk-client-api-create)\.

When updating the queue destination in a queue, there is a short transition period \(up to 30 seconds\) during which game sessions placed on the queue destinations may still end up on the old fleet\. 

## To create a queue<a name="queues-creating-console"></a>

------
#### [ Console ]

Game session queues that are created using the Console have the following characteristics:
+ Game sessions can be placed in any location that is part of any destination fleet in the queue\. If you want to exclude one or more locations, use the AWS CLI to update a queue, using [update\-game\-session\-queue](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-game-session-queue.html), to add a filter configuration\.
+ FleetIQ automatically applies the default prioritization when placing game sessions\. If you want to customize how your queue handles prioritization, use the AWS CLI to update a queue, using [update\-game\-session\-queue](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-game-session-queue.html), to add a priority configuration\.

1. Open the GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/), and choose the region you want to create your queue in\.

1. From the GameLift menu, choose **Create a queue**\. 

1. On the **Create queue** page, complete the **Queue Details** section: 
   + **Queue Name** – Create a meaningful queue name so you can identify it in a list and in metrics\. Requests for new a game session \(using [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html)\) must specify a queue by this name\. Spaces and special characters are not allowed\. 
   + **Queue Timeout** – Specify how long you want GameLift to try to place a new game session before stopping\. GameLift continues to search for available resources on any fleet until the request times out\. 

1. Under **Player latency policies**, define zero or more policies for the queue\. For each placement request, GameLift automatically minimizes the average latency across all players\. You can also create a latency policies to set a maximum limit for each individual player\. Player latency policies are evaluated only when player latency data is provided in the placement request\. You can opt to enforce one limit throughout the placement process, or you can gradually relax the limit over time\. Learn more at [Create a player latency policy](queues-design.md#queues-design-latency)\.

   1. To add a first policy, choose **Add player latency policy**\. Enter a maximum player latency value for this policy \(default is 150 milliseconds\)\. As indicated in the policy language, this first policy will be enforced either for the entire placement process or—if you create additional policies—for any remaining time after the other policies have expired\. 

   1. To add another player latency policy, choose **Add player latency policy** again\. For additional policies, set the maximum player latency value and a length of time \(in seconds\) to enforce it\. Maximum latency values for these policies must be lower than the first policy\. 

      As you add policies, the console automatically reorders the polices based on the maximum player latency value, lowest value listed first\. This is the order in which the policies are enforced during a game session placement effort\. 

1. Under **Destinations**, add one or more destinations to the queue\. A queue can contain fleets from multiple regions and with both on\-demand and spot fleets\. All fleets in the queue must have the same certificate configuration \(either GENERATED or DISABLED\)\. All fleets should be running game builds that are compatible with the game clients that will use the queue, such as fleets in multiple regions that are running the same game build\. Fleets and aliases must exist before you can add them as destinations\.

   1. Choose **Add destination**\.

   1. Use the columns to specify the region and type \(fleet or alias\) for your destination\. From the resulting list of fleet or alias names, select the one you want to add\.

   1. To save the destination, choose the green checkmark icon\. You must save each destination before adding another one, changing the default order, or saving the queue\.

   1. If you have multiple destinations, set the default order by using the arrow icons in the **Priority \(default\)** column\. This order is used by GameLift when searching destinations for available resources to place a new game session\. \(The default order is overridden if a game session placement request includes player latency data\.\) 

1. Optionally, add the ARN for an SNS topic where you want to receive placement\-related event notifications\. Learn more about [Set up event notification for game session placement](queue-notification.md)\.

1. Once you've finished configuring your new queue, choose **Create queue**\. Your new queue is saved and the **Queues** page shows the new queue and any other queues that exist\. You can choose a queue from this page to view detailed information, including queue metrics\. You can edit a queue configuration at any time\.

------
#### [ AWS CLI ]

You can use the AWS Command Line Interface \(AWS CLI\) to create a queue\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

**To create a queue**
+ Open a command line window and use the `create-game-session-queue` command to define a new queue\. For more information, see the [AWS CLI command reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-game-session-queue.html)\.

The following example creates a game session queue that has two fleet destinations and allows up to five minutes for the placement process\. Fleets are listed as destinations and identified by either a fleet ARN or alias ARN\. All fleets and aliases must already exist\. The queue is configured with a filter configuration, a custom priority configuration, and a notification target for tracking placement events\. The priority configuration looks at hosting cost first, followed location, applied by the defined order\.

**Note**  
You can get fleet and alias ARN values by calling either [describe\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-attributes.html) or [describe\-alias](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-alias.html) with the fleet or alias ID\. For more information on ARN \(Amazon Resource Name\) formats, see [ARNs and AWS service namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

```
$ AWS gamelift create-game-session-queue 
--name "Sample test queue"
--timeout-in-seconds 300
--destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910
               DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901
--filter-configuration "AllowedLocations=us-east-1, ca-central-1, us-east-2, us-west-2"
--priority-configuration "PriorityOrder=COST,LOCATION LocationOrder=us-east-2,ca-central-1,us-west-2,us-east-1"
--notification-target "arn:aws:sns:us-west-2:111122223333:My_Placement_SNS_Topic"
```

*Copiable version:*

```
AWS gamelift create-game-session-queue --name "Sample test queue" --timeout-in-seconds 300 --destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910 DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901 --filter-configuration "AllowedLocations=us-east-1, ca-central-1, us-east-2, us-west-2" --priority-configuration "PriorityOrder=COST,LOCATION LocationOrder=us-east-2,ca-central-1,us-west-2,us-east-1" --notification-target "arn:aws:sns:us-west-2:111122223333:My_Placement_SNS_Topic"
```

If the `create-game-session-queue` request is successful, GameLift returns a [GameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionQueue.html) object with the new queue configuration\. You can now submit requests to the queue using [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html)\. 

**To create a queue with player latency policies**
+ Open a command line window and use the `create-game-session-queue` command to define a new queue\. For more information, see the [AWS CLI command reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-game-session-queue.html)\.

The following example creates a queue with a 10\-minute timeout, three destinations, and a set of player latency policies\. In this example, the first player latency policy is in force for the first two minutes, the second policy is in force for the third and fourth minute, and the third policy is in force for six minutes until the placement request times out\. 

```
$ AWS gamelift create-game-session-queue
--name "matchmaker-queue"
--timeout-in-seconds 600
--destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910
               DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901
               DestinationArn=arn:aws:gamelift:us-west-2::fleet/fleet-f1234567-b8c9-0d1e-2fa3-b45c6d7e8912
--player-latency-policies "MaximumIndividualPlayerLatencyMilliseconds=50,PolicyDurationSeconds=120"
                          "MaximumIndividualPlayerLatencyMilliseconds=100,PolicyDurationSeconds=120"
                          "MaximumIndividualPlayerLatencyMilliseconds=150"
```

*Copiable version:*

```
AWS gamelift create-game-session-queue --name "matchmaker-queue" --timeout-in-seconds 600 --destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910 DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901 DestinationArn=arn:aws:gamelift:us-west-2::fleet/fleet-f1234567-b8c9-0d1e-2fa3-b45c6d7e8912 --player-latency-policies "MaximumIndividualPlayerLatencyMilliseconds=50,PolicyDurationSeconds=120" "MaximumIndividualPlayerLatencyMilliseconds=100,PolicyDurationSeconds=120" "MaximumIndividualPlayerLatencyMilliseconds=150"
```

If the `create-game-session-queue` request is successful, GameLift returns a [GameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionQueue.html) object with the new queue configuration\. 

------