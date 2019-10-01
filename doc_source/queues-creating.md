# Create a Queue<a name="queues-creating"></a>

Queues are used to place new game sessions with the best available hosting resources across multiple fleets and regions\. To learn more about building queues for your game, see [Design a Game Session Queue](queues-design.md)\.

In a game client, new game sessions are started with queues by using placement requests\. Learn more about game session placement in [Create Game Sessions](gamelift-sdk-client-api.md#gamelift-sdk-client-api-create) 

To create a queue, you can use either the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/) or the AWS Command Line Interface \(CLI\)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

## Create a Queue \(Console\)<a name="queues-creating-console"></a>

Create a queue from the AWS Management Console\.

**To create a queue**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/), and choose the region you want to create your queue in\.

1. From the Amazon GameLift menu, choose **Create a queue**\. 

1. On the **Create queue** page, complete the **Queue Details** section: 
   + **Queue Name** – Create a meaningful queue name so you can identify it in a list and in metrics\. Requests for new a game session \(using [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html)\) must specify a queue by this name\. Spaces and special characters are not allowed\. 
   + **Queue Timeout** – Specify how long you want Amazon GameLift to try to place a new game session before stopping\. Amazon GameLift continues to search for available resources on any fleet until the request times out\. 

1. Under **Player latency policies**, define zero or more policies for the queue\. For each placement request, Amazon GameLift automatically minimizes the average latency across all players\. You can also create a latency policies to set a maximum limit for each individual player\. Player latency policies are evaluated only when player latency data is provided in the placement request\. You can opt to enforce one limit throughout the placement process, or you can gradually relax the limit over time\. Learn more at [Design Player Latency Policies](queues-design.md#queues-design-latency)\.

   1. To add a first policy, choose **Add player latency policy**\. Enter a maximum player latency value for this policy \(default is 150 milliseconds\)\. As indicated in the policy language, this first policy will be enforced either for the entire placement process or—if you create additional policies—for any remaining time after the other policies have expired\. 

   1. To add another player latency policy, choose **Add player latency policy** again\. For additional policies, set the maximum player latency value and a length of time \(in seconds\) to enforce it\. Maximum latency values for these policies must be lower than the first policy\. 

      As you add policies, the console automatically reorders the polices based on the maximum player latency value, lowest value listed first\. This is the order in which the policies are enforced during a game session placement effort\. 

1. Under **Destinations**, add one or more destinations to the queue\. A queue can contain fleets from multiple regions and with both on\-demand and spot fleets\. All fleets in the queue must have the same certificate configuration \(either GENERATED or DISABLED\)\. All fleets should be running game builds that are compatible with the game clients that will use the queue, such as fleets in multiple regions that are running the same game build\. Fleets and aliases must exist before you can add them as destinations\.

   1. Choose **Add destination**\.

   1. Use the columns to specify the region and type \(fleet or alias\) for your destination\. From the resulting list of fleet or alias names, select the one you want to add\.

   1. To save the destination, choose the green checkmark icon\. You must save each destination before adding another one, changing the default order, or saving the queue\.

   1. If you have multiple destinations, set the default order by using the arrow icons in the **Priority \(default\)** column\. This order is used by Amazon GameLift when searching destinations for available resources to place a new game session\. \(The default order is overridden if a game session placement request includes player latency data\.\) 

1. Once you've finished configuring your new queue, choose **Create queue**\. Your new queue is saved and the **Queues** page shows the new queue and any other queues that exist\. You can choose a queue from this page to view detailed information, including queue metrics\. You can edit a queue configuration at any time\.

## Create a Queue \(AWS CLI\)<a name="queues-creating-cli"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to create a queue\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

**To create a queue**
+ Open a command line window and use the `create-game-session-queue` command to define a new queue\. For more information, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-game-session-queue.html)\.

The following example creates a queue for placing new game sessions on any of several fleets before timing out at five minutes\. Fleets are listed as destinations and identified by either a fleet ARN or alias ARN\. All fleets and aliases must already exist\. Amazon GameLift tries to place new game sessions on fleets in the order they are listed here unless the order is overridden by individual game session placement requests\.

**Note**  
You can get fleet and alias ARN values by calling either [describe\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-attributes.html) or [describe\-alias](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-alias.html) with the fleet or alias ID\. For more information on ARN \(Amazon Resource Name\) formats, see [ARNs and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

```
$ aws gamelift create-game-session-queue 
--name "Sample test queue"
--timeout-in-seconds 300
--destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910
               DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901
               DestinationArn=arn:aws:gamelift:us-west-2::fleet/fleet-f1234567-b8c9-0d1e-2fa3-b45c6d7e8912
```

*Copiable version:*

```
aws gamelift create-game-session-queue --name "Sample test queue" --timeout-in-seconds 300 --destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910                DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901                DestinationArn=arn:aws:gamelift:us-west-2::fleet/fleet-f1234567-b8c9-0d1e-2fa3-b45c6d7e8912
```

If the `create-game-session-queue` request is successful, Amazon GameLift returns a [GameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionQueue.html) object with the new queue configuration\. You can now submit requests to the queue using [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference//API_StartGameSessionPlacement.html)\. 

**To create a queue with player latency policies**
+ Open a command line window and use the `create-game-session-queue` command to define a new queue\. For more information, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-game-session-queue.html)\.

The following example creates a queue with a 10\-minute timeout, three destinations, and a set of player latency policies\. In this example, the first player latency policy is in force for the first two minutes, the second policy is in force for the third and fourth minute, and the third policy is in force for six minutes until the placement request times out\. 

```
$ aws gamelift create-game-session-queue
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
aws gamelift create-game-session-queue --name "matchmaker-queue" --timeout-in-seconds 600 --destinations DestinationArn=arn:aws:gamelift:us-east-1::alias/alias-a1234567-b8c9-0d1e-2fa3-b45c6d7e8910 DestinationArn=arn:aws:gamelift:us-west-2::alias/alias-b0234567-c8d9-0e1f-2ab3-c45d6e7f8901 DestinationArn=arn:aws:gamelift:us-west-2::fleet/fleet-f1234567-b8c9-0d1e-2fa3-b45c6d7e8912 --player-latency-policies "MaximumIndividualPlayerLatencyMilliseconds=50,PolicyDurationSeconds=120" "MaximumIndividualPlayerLatencyMilliseconds=100,PolicyDurationSeconds=120" "MaximumIndividualPlayerLatencyMilliseconds=150"
```

If the `create-game-session-queue` request is successful, Amazon GameLift returns a [GameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionQueue.html) object with the new queue configuration\. 