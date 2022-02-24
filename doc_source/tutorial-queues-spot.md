# Tutorial: Set up a game session queue for Spot Instances<a name="tutorial-queues-spot"></a>

**Introduction**  
This tutorial describes how to set up game session placement for games that are deployed on low\-cost Spot fleets\. GameLift uses queues to field game session placement requests, locate available game servers to host them, and start the new game sessions\. When building queues for Spot fleets, you need to take additional steps to minimize Spot interruptions and maintain continual game server availability for your players\. This tutorial follows best practices and provides tips on building queues for Spot fleets\. 

**Intended audience**  
This tutorial is for game developers who use GameLift Spot fleets to host a custom game server or Realtime Servers and need to set up game session placement\. This tutorial assumes that you have a basic understanding of [How GameLift works](gamelift-howitworks.md)\. 

**What you'll learn**  
This tutorial outlines the key decision points and steps required to build a game session placement layer for Spot fleets\. In it, you will learn how to do the following:  
+ Define the group of players \(scope\) who will be served by your game session queue\.
+ Build a fleet infrastructure to support the game session queue's scope\.
+ Assign an alias to each fleet to abstract the fleet ID\.
+ Create a queue, add fleets, and prioritize where game sessions are placed\.
+ Add player latency policies to help minimize latency issues for every player\.

**Prerequisites**  
Before creating fleets and queues for game session placement, you should complete the following tasks:   
+ Complete the required [game server integration](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-intro.html) tasks\. Your game server must be able to receive prompts from the GameLift service to start and stop game sessions\. 
+ [Upload](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-build-intro.html) your game server build or Realtime script to the GameLift service for deployment\.
+ Plan your fleet configuration\. At a minimum, you need to know which [instance types](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html#gamelift-CreateFleet-request-EC2InstanceType) you want to use \(choose at least one, preferably two or three\), and whether your fleets require an [Instance role ARN](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html#gamelift-CreateFleet-request-InstanceRoleArn) and/or a [TLS certificate](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html#gamelift-CreateFleet-request-CertificateConfiguration)\. These properties can't be changed after a fleet is created\.

## Step 1: Define the scope of your queue<a name="tutorial-queues-spot-segment"></a>

Your game's player population might have groups of players who will never play together in the same game sessions\. For example, if your game is published in two languages, you need to place one player segment into Japanese game servers and another segment into English game servers\. 

To set up game session placement for your player population, you create a separate queue for each player segment\. Each queue must be scoped appropriately in order to place players into the correct game servers\. Some common ways that queues are scoped include:
+ By geographic locations\. When deploying your game servers in multiple geographic areas, you might build separate queues for players in each smaller area to limit the impact on player latency\.
+ By build/script variations\. If you have more than one variation of your game server, you might be supporting player groups that can't play in the same game sessions\. For example, game server builds/scripts might support different languages, device types, etc\.
+ By event types\. You might create a special queue to manage games for participants in tournaments or other special events\. 

**Tutorial example**  
In this tutorial, we are designing a queue for a game that has only one game server build variation\. For launch, the game is being released only in two Asia Regions: ap\-northeast\-2 \(Seoul\) and ap\-southeast\-1 \(Singapore\)\. Because these Regions are relatively close to each other, latency isn't a significant issue for our player population\. For this example, we have only one player segment, which means we need to create just one queue\. At some future point, when the game is being released in North America, we'll create a second queue that is scoped for North American players\.

## Step 2: Create Spot fleet infrastructure<a name="tutorial-queues-spot-fleet"></a>

Build a set of fleets to host games for your player segment\. Create fleets in Regions and with game server builds/scripts that fit the scope that you defined in Step 1\. Your infrastructure should fit the following criteria:
+ **Create fleets in at least two Regions\.** It's possible for a Region to have a temporary outage\. By having game servers hosted in at least one other region, you mitigate the impact of regional outages on your players\. Even if you're supporting a player base in only one Region, place back\-up fleets in a second Region nearby\. You can keep your back\-up fleets scaled down, and use auto scaling to increase capacity if usage increases\.
+ **Create at least one On\-Demand fleet in each Region\.** In each Region, create at least one Spot fleet and one On\-Demand fleet\. Since Spot fleets can become unavailable on short notice, On\-Demand fleets provide back\-up game servers for your players\. You can keep your backup fleets scaled down until they're needed, and use auto scaling to increase On\-Demand capacity when Spot fleets are unavailable\.
+ **Select different instance types across multiple Spot fleets in a Region\.** If you are creating more than one Spot fleet in a region, vary the instance types of the fleets\. If one Spot Instance type becomes temporarily unavailable, the interruption affects only one Spot fleet in the Region\. Best practice is to choose widely available instance types, and use instance types in the same family \(for example, m5\.large, m5\.xlarge, m5\.2xlarge\)\. Use the [GameLift console](https://console.aws.amazon.com/gamelift/) to check historical pricing data for instance types\. 
+ **Use the same or a similar game build or script for all fleets\.** The queue might put players into game sessions on any fleet in the queue\. Players must be able to play in any game session on any fleet\. 
+ **Use the same TLS certificate setting for all fleets\.** Game clients that connect to game sessions in your fleets need to have compatible communication protocols\. 

When you're ready to build your fleets, see [Deploy a GameLift fleet with a custom game build](fleets-creating.md) or [Deploy a Realtime Servers Fleet](realtime-fleets-creating.md) for detailed instructions on using the GameLift console or the AWS CLI to create new fleets\.

**Tip**  
We highly recommend including the fleet type \(Spot or On\-demand\) in your fleet names\. This makes it much easier to identify fleet types when viewing a list of fleets\.

**Tutorial example**  
As determined in Step 1, our queue needs to support players in two Regions\. We need to create a two\-Region infrastructure with at least one Spot fleet and one On\-Demand fleet in each Region\. Every fleet deploys the same game server build\. In addition, we anticipate that player traffic will be heavier in the Seoul Region, so we want to double up on our Spot fleets there\. By adding another Spot fleet with a different instance type, we increase the likelihood that games in that Region will be hosted on low\-cost Spot instances even if Spot interruptions occur\. 

Our resulting fleet infrastructure includes five fleets in total:

 ap\-northeast\-2 \(Seoul\)
+ Spot fleet, c5\.large instance type
+ Spot fleet, c5\.xlarge instance type
+ On\-Demand fleet, c5\.large instance type 

ap\-southeast\-1 \(Singapore\)
+ Spot fleet, c5\.large instance type
+ On\-Demand fleet, c5\.large instance type

![\[Diagram shows the example Spot fleet infrastructure, with 3 fleets in the ap-northeast-2 (Seoul) Region and 2 fleets in the ap-southeast-1 (Singapore) Region.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step2-vsd.png)

## Step 3: Assign aliases for each fleet<a name="tutorial-queues-spot-alias"></a>

Create a new alias for each fleet in your infrastructure\. Aliases are used to abstract fleet identities; because fleets need to be replaced periodically for a variety of reasons, aliases make this process simple and efficient\. When your fleets are created, see [Add an alias to a GameLift fleet](aliases-creating.md) for instructions on using the GameLift console or the AWS CLI to create new aliases and point them to the appropriate fleet\. You must create the alias in the same Region as the fleet it will point to\.

**Tip**  
We highly recommend including the fleet type \("spot" or "on\-demand"\) in your alias names\. This makes it much easier to identify the fleet type when viewing a list of aliases\.

**Tutorial example**  
Our fleet infrastructure has five fleets, so we need to create five aliases, choosing the "simple" routing strategy and selecting one of our five fleets\. We need three aliases in the ap\-northeast\-2 \(Seoul\) Region, and two aliases in the ap\-southeast\-1 \(Singapore\) Region\.

![\[Diagram shows the example Spot fleet infrastructure with aliases added for each fleet.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step3-vsd.png)

## Step 4: Create a queue with destinations<a name="tutorial-queues-spot-queue"></a>

With your fleets and aliases prepared, you've done the hard part of setting up the infrastructure for game session placement\. Now you need to create the game session queue itself and add your fleet destinations\. See [Create a game session queue](queues-creating.md) for detailed instructions on using the GameLift console or the AWS CLI to create your new queue\. 

When creating your queue:
+ Keep the default timeout of 10 minutes to start with\. You'll want to test out how your queue timeout affects your players' wait times for getting into games\. 
**Tip**  
You'll want to monitor your game's queue timeout rate\. It's a good indicator when something is slowing down your game session placement pipeline\.
+ Skip the section on player latency policies for now\. We'll cover this in the next step\.
+ Prioritize the fleets in your queue\. Fleet prioritization determines where the queue looks first when searching for available resources to host a new game session\. You might choose to prioritize by Region, instance types, fleet type, and so on\. When working with Spot fleets, we recommend either of the following approaches:
  + If your infrastructure uses a primary Region with fleets in a second Region for back\-up only, you want to prioritize fleets first by region, and then by fleet type\. With this approach, all fleets in the primary Region are placed at the top of the list, with Spot fleets followed by On\-Demand fleets\. 
  + If your infrastructure uses multiple Regions equally, you want to prioritize fleets by fleet type, placing Spot fleets at the top of the list\.
**Note**  
When a game session placement request contains player latency information, your default prioritization may change\. In this case GameLift FleetIQ re\-prioritizes the queue's fleets by Region and tries to place game sessions where players are reporting the lowest latency\.

**Tutorial example**  
For this example, we create a new queue with the name "MBG\_ASIA\_spot", and add the aliases for all five of our fleets\. Our game session requests do include latency information, but we still need to implement a prioritization strategy\. For this queue, we prioritize placements first by Region and second by fleet type\. The following list details the queue destinations in prioritized order: 
+ Alias for ap\-northeast\-2 \(Seoul\), c5\.large Spot fleet
+ Alias for ap\-northeast\-2 \(Seoul\), c5\.xlarge Spot fleet
+ Alias for ap\-northeast\-2 \(Seoul\), c5\.large On\-Demand fleet
+ Alias for ap\-southeast\-1 \(Singapore\), c5 large Spot fleet
+ Alias for ap\-southeast\-1 \(Singapore\), c5 large On\-Demand fleet

Based on this configuration, this queue will always attempt to place new game sessions into a Spot fleet in Seoul\. When those fleets are full, the queue will use available capacity on the Seoul On\-Demand fleet as a backup\. Only once all three Seoul fleets are unavailable will game sessions be placed on the Singapore fleets\.

![\[Diagram shows the example queue with prioritized destinations.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step4-vsd.png)

## Step 5: Add latency limits to the queue<a name="tutorial-queues-spot-latency"></a>

If your game includes regional player latency information in game session placement requests, FleetIQ works to place players into game sessions with the lowest possible latency experience\. For game session requests that include multiple players, FleetIQ uses the average latency values of all players in the request\. While this approach works most of the time, it occasionally puts one or more of the players into a game session with unacceptable latency\. 

To handle these outlier cases, you can set up a player latency policy in your queue to enforce a hard latency limit for all individual players in a game session placement request\. This policy ensures that no individual player gets put into a game session with an unreasonably high latency\. A latency policy can increase player wait times for game session placements, as the queue searches for an acceptable game server, or it can prevent a placement altogether\. Your policy can also include relaxable rules that increase the maximum allowed latency over time\. See [Create a player latency policy](queues-design.md#queues-design-latency) for additional help with player latency policies\.

**Tip**  
If you want to manage player latency more specifically, such as to require similar latency across all players in a group, you can use [GameLift FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/) and create latency\-based matchmaking rules\.

**Tutorial example**  
Our game includes latency information in our game session placement requests, and we have a player party feature that creates a game session for a group of players\. We are willing to have players wait a little longer to get into games with the best possible gameplay experience\. Our game tests show that latencies under 50 milliseconds are optimal, and the game becomes unplayable at latencies over 250 milliseconds\. Players' tolerance for wait times starts to fall off at about one minute\. We can use this information to set the parameters of our player latency policy\.

The GameLift console provides a simple UI for building player latency policy statements\. For our queue, which has a five\-minute timeout \(300 seconds\), we add the following policy statements\. \(You can also set player latency policies using the AWS CLI command [update\-game\-session\-queue](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-game-session-queue.html)\.\)

1. Spend 60 seconds searching for a destination where all player latencies are less than 50 milliseconds, then\.\.\. 

1. Spend 30 seconds searching for a destination where all player latencies are less than 125 milliseconds, then\.\.\. 

1. Spend the remaining queue timeout searching for a destination where all player latencies are less than 250 milliseconds\. 

With this policy, our queue looks for placements with optimal latency \(under 50ms\) for the first minute, and then relaxes the limit fairly quickly after that\. The queue does not make any placements where one player would have a latency of 250ms or higher\. 

![\[Diagram shows the example queue with player latency policies added.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step5-vsd.png)

## Summary<a name="tutorial-queues-spot-summary"></a>

Congratulations\! If you've followed the steps in this tutorial, you now have a game session queue that is scoped for a particular segment of your player population\. It uses Spot fleets effectively and is resilient when rare Spot interruptions occur\. In addition, your queue's fleets are strategically prioritized and you've added hard latency limits to protect players from bad gameplay experiences\.

You can now use the queue to place game sessions for the player segment it serves\. When making game session placement requests for these players, you must reference this game session queue name in the request\. For more information on making game session placement requests, see [Create Game Sessions](gamelift-sdk-client-api.md#gamelift-sdk-client-api-create) or [Integrating a Game Client for Realtime Servers](realtime-client.md)\.