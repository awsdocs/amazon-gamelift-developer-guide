# Create a game session queue<a name="queues-creating"></a>

Queues are used to place new game sessions with the best available hosting resources across multiple fleets and regions\. To learn more about building queues for your game, see [Design a game session queue](queues-design.md)\.

In a game client, new game sessions are started with queues by using placement requests\. Learn more about game session placement in [Create game sessions](gamelift-sdk-client-api.md#gamelift-sdk-client-api-create)\.

When updating the queue destination in a queue, there is a short transition period \(up to 30 seconds\) during which game sessions placed on the queue destinations may still end up on the old fleet\. 

------
#### [ Console ]

1. In the [GameLift console](https://console.aws.amazon.com/gamelift/), in the navigation page, choose **Queues**\.

1. On the **Queues** page, choose **Create queue**\. 

1. On the **Create queue** page, under **Queue settings** do the following:

   1. For **Name**, enter a queue name\.

   1. For **Timeout**, enter long you want GameLift to try to place a game session before stopping\. GameLift searches for available resources on any fleet until the request times out\.

   1. \(Optional\) For **Player latency policies**, enter how long GameLift should look for resources within the defined maximum latency\. Add additional policies to gradual relax the maximum latency\. To add additional policies, choose **Add policy**\.

1. Under **Game session placement locations**, select locations to include in the queue\. By default **All locations** are included\. All fleets in the queue must have the same certificate configuration\. All fleets should be running game builds that are compatible with the game clients using the queue\.

1. Under **Destination order**, add one or more destinations to the queue\.

   1. Choose **Add destination**\.

   1. Select the **Location** that the destination is in\.

   1. Select the type for your destination\.

   1. From the resulting list of fleet or alias names, select the one you want to add\.

   1. If you have multiple destinations, set the default order by dragging the six dots icon to the left of the destination\. GameLift uses this order when searching destinations for available resources to place a new game session\. 

1. For **Game session placement priority**, add and drag the **Latency**, **Cost**, **Destination**, and **Location** values to define how GameLift prioritizes fleets in your queue\. For more information about prioritizing fleets, see [Prioritize where game sessions are placed](queues-design.md#queues-design-priority)\.

1. Add locations to your **Location order** and drag them to the priority that the queue should use\. If **Location** is the last priority for game session placement, GameLift uses it as a tiebreaker\.

1. \(Optional\) Under **Event notification settings** do the following:

   1. Select or create an SNS topic to receive placement\-related event notifications\. For more information about event notifications, see [Set up event notification for game session placement](queue-notification.md)\.

   1. Add **Custom event data** to append to events created by this queue\.

1. \(Optional\) Add **Tags**\. For more information about tagging, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

1. Choose **Create**\.

------
#### [ AWS CLI ]

**Example Create a queue**  
The following example creates a game session queue with these configurations:  
+ A five minute timeout
+ Two fleet destinations
+ Filters to only allow locations in the `us-east-1`, `us-east-2`\. `us-west-2`, and `ca-central-1`
+ Prioritizes destinations based on cost and then locations in the defined order\.

```
aws gamelift create-game-session-queue \
    --name "sample-test-queue" \
    --timeout-in-seconds 300 \
    --destinations DestinationArn="arn:aws:gamelift:us-east-1:111122223333:fleet/fleet-772266ba-8c82-4a6e-b620-a74a62a93ff8" DestinationArn="arn:aws:gamelift:us-east-1:111122223333:fleet/fleet-33f28fb6-aa8b-4867-85b4-ceb217bf5994" \
    --filter-configuration "AllowedLocations=us-east-1, ca-central-1, us-east-2, us-west-2" \
    --priority-configuration PriorityOrder="LOCATION","DESTINATION",LocationOrder="us-east-1","us-east-2","ca-central-1","us-west-2" \
    --notification-target "arn:aws:sns:us-east-1:111122223333:gamelift-test.fifo"
```

**Note**  
You can get fleet and alias ARN values by calling either [describe\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-attributes.html) or [describe\-alias](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-alias.html) with the fleet or alias ID\.

If the `create-game-session-queue` request is successful, GameLift returns a [GameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionQueue.html) object with the new queue configuration\. You can now submit requests to the queue using [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html)\. 

**Example Create a queue with player latency policies**  
The following example creates a game session queue with these configurations:  
+ A ten minute timeout
+ Three fleet destinations
+ A set of player latency policies

```
aws gamelift create-game-session-queue \
    --name "matchmaker-queue" \
    --timeout-in-seconds 600 \
    --destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910 \
               DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901 \
               DestinationArn=arn:aws:gamelift:us-west-2::fleet/fleet-f1234567-b8c9-0d1e-2fa3-b45c6d7e8912 \
    --player-latency-policies "MaximumIndividualPlayerLatencyMilliseconds=50,PolicyDurationSeconds=120" \
               "MaximumIndividualPlayerLatencyMilliseconds=100,PolicyDurationSeconds=120" \
               "MaximumIndividualPlayerLatencyMilliseconds=150" \
```

If the `create-game-session-queue` request is successful, GameLift returns a [GameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionQueue.html) object with the new queue configuration\. 

------