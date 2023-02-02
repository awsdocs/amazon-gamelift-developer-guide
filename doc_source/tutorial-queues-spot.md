# Tutorial: Set up a game session queue for Spot instances<a name="tutorial-queues-spot"></a>

**Introduction**  
This tutorial describes how to set up game session placement for games deployed on low\-cost Spot fleets\. GameLift uses queues to field game session placement requests, locate available game servers to host them, and start new game sessions\. Spot fleets require additional steps to maintain continual game server availability for your plaers\.

**Intended audience**  
This tutorial is for game developers who use GameLift Spot fleets to host a custom game server or Realtime Servers and want to set up game session placement\. This tutorial assumes that you have a basic understanding of [How GameLift works](gamelift-howitworks.md)\. 

**What you'll learn**  
+ Define the group of players who your game session queue servers\.
+ Build a fleet infrastructure to support the game session queue's scope\.
+ Assign an alias to each fleet to abstract the fleet ID\.
+ Create a queue, add fleets, and prioritize where GameLift places game sessions\.
+ Add player latency policies to help minimize latency issues\.

**Prerequisites**  
Before creating fleets and queues for game session placement, complete the following tasks:   
+ [Integrate you game server with GameLift\.](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-intro.html)
+ [Upload your game server](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-build-intro.html) build or Realtime script to GameLift\.
+ [Plan your fleet configuration\.](https://docs.aws.amazon.com/gamelift/latest/developerguide/fleets-design.html) 

## Step 1: Define the scope of your queue<a name="tutorial-queues-spot-segment"></a>

Your game's player population might have groups of players who shouldn't play together\. For example, if you publish your game in two languages, you would place one player segment into Japanese game servers and another segment into English game servers\. 

To set up game session placement for your player population, create a separate queue for each player segment\. Scope each queue to place players into the correct game servers\. Some common ways to scope queues include:
+ **By geographic locations\.** When deploying your game servers in multiple geographic areas, you might build queues for players in each location to reduce player latency\.
+ **By build or script variations\.** If you have more than one variation of your game server, you might be supporting player groups that can't play in the same game sessions\. For example, game server builds or scripts might support different languages, device types, etc\.
+ **By event types\.** You might create a special queue to manage games for participants in tournaments or other special events\. 

**Tutorial example**  
In this tutorial, we design a queue for a game that has one game server build variation\. At launch, we're releasing the game in two locations: ap\-northeast\-2 \(Seoul\) and ap\-southeast\-1 \(Singapore\)\. Because these locations are close to each other, latency isn't an issue for our players\. For this example, we have only one player segment, which means we create one queue\. In the future, when we release the game in North America, we can create a second queue that's scoped for North American players\.

## Step 2: Create Spot fleet infrastructure<a name="tutorial-queues-spot-fleet"></a>

Create fleets in locations and with game server builds or scripts that fit the scope that you defined in Step 1\. Your infrastructure should fit the following criteria:
+ **Create fleets in at least two locations\.** By having game servers hosted in at least one other location, you mitigate the impact of Regional outages on your players\. You can keep your back\-up fleets scaled down, and use auto\-scaling to increase capacity if usage increases\.
+ **Create at least one On\-Demand fleet in each location\.** On\-Demand fleets provide back\-up game servers for your players\. You can keep your backup fleets scaled down until they're needed, and use auto\-scaling to increase On\-Demand capacity when Spot fleets are unavailable\.
+ **Select different instance types across multiple Spot fleets in a location\.** If one Spot Instance type becomes temporarily unavailable, the interruption affects only one Spot fleet in the location\. Best practice is to choose widely available instance types, and use instance types in the same family \(for example, m5\.large, m5\.xlarge, m5\.2xlarge\)\. Use the [GameLift console](https://console.aws.amazon.com/gamelift/) to view historical pricing data for instance types\. 
+ **Use the same or a similar game build or script for all fleets\.** The queue might put players into game sessions on any fleet in the queue\. Players must be able to play in any game session on any fleet\. 
+ **Use the same TLS certificate setting for all fleets\.** Game clients that connect to game sessions in your fleets must have compatible communication protocols\. 

When you're ready to build your fleets, see [Create a managed fleet](fleets-creating.md) for detailed instructions on creating new fleets\.

**Tutorial example**  
We create a two location infrastructure with at least one Spot fleet and one On\-Demand fleet in each location\. Every fleet deploys the same game server build\. In addition, we anticipate that player traffic will be heavier in the Seoul location, so we add more Spot fleets there\.

![\[Diagram shows the example Spot fleet infrastructure, with 3 fleets in the ap-northeast-2 (Seoul) location and 2 fleets in the ap-southeast-1 (Singapore) location. All instances in both fleets are using the build MBG_prod_V1. The fleet in ap-northeast-2 contains the following fleet configurations: fleet 1234_spot_1 with an instance type of c5.large, fleet 1234_spot_2 with an instance type of c5.xlarge, and fleet 1234_ondemand with an instance type of c5.large. The fleet in ap-southeast-1 contains the following fleet configurations: fleet 1234_spot_1 with an instance type of c5.large and fleet 1234_ondemand with an instance type of c5.large.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step2.png)

## Step 3: Assign aliases for each fleet<a name="tutorial-queues-spot-alias"></a>

Create a new alias for each fleet in your infrastructure\. Aliases abstract fleet identities making periodic fleet replacement simple and efficient\. For more information about creating aliases, see [Add an alias to a GameLift fleet](aliases-creating.md)\.

**Tutorial example**  
Our fleet infrastructure has five fleets, so we create five aliases using the simple routing strategy\. We need three aliases in the ap\-northeast\-2 \(Seoul\) location, and two aliases in the ap\-southeast\-1 \(Singapore\) location\.

![\[Diagram shows the example Spot fleet infrastructure described in step two with aliases added to each fleet. Fleet 1234_spot_1 has the alias MBG_spot_1, Fleet 1234_spot_2 has the alias MBG_spot_2, and fleet 1234_ondemand has the alias MBG_ondemand.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step3.png)

## Step 4: Create a queue with destinations<a name="tutorial-queues-spot-queue"></a>

Create the game session queue and add your fleet destinations\. For more information about creating a queue, see [Create a game session queue](queues-creating.md)\. 

When creating your queue:
+ Set the default timeout to 10 minutes\. Later, you can test how the queue timeout affects your players' wait times for getting into games\. 
**Tip**  
Monitor your game's queue timeout rate, it's a good indicator of when something is slowing down your game session placement pipeline\.
+ Skip the section on player latency policies for now\. We'll cover this in the next step\.
+ Prioritize the fleets in your queue\. Fleet prioritization determines where the queue looks first when searching for available resources to host a new game session\. When working with Spot fleets, we recommend either of the following approaches:
  + If your infrastructure uses a primary location with fleets in a second location for back\-up, prioritize fleets first by location then by fleet type\. With this approach, GameLift places fleets in the primary location at the top of the queue, with Spot fleets followed by On\-Demand fleets\. 
  + If your infrastructure uses multiple locations equally, prioritize fleets by fleet type, placing Spot fleets at the top of the queue\.
**Note**  
When a game session placement request contains player latency information, FleetIQ re\-prioritizes the queue's fleets by location and tries to place game sessions where players are reporting the lowest latency\. For more information about FleetIQ, see [How GameLift queues works](queues-intro.md#queues-design-fleetiq)\.

**Tutorial example**  
For this example, we create a new queue with the name **MBG\_spot\_queue**, and add the aliases of all five of our fleets\. We then prioritize placements first by location and second by fleet type\. 

Based on this configuration, this queue always attempts to place new game sessions into a Spot fleet in Seoul\. When those fleets are full, the queue uses available capacity on the Seoul On\-Demand fleet as a backup\. If all three Seoul fleets are unavailable GameLift places game sessions on the Singapore fleets\.

![\[Diagram shows the example queue with a timeout of 300 seconds and prioritized destinations. The destination are in the following order: 1234_spot_1 in ap-northeast-2, 1234_spot_2 in ap-northeast-2, 1234_ondemand in ap-northeast-2, 1234_spot_1 in ap-southeast-1, and 1234_ondemand in ap-southeast-1.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step4.png)

## Step 5: Add latency limits to the queue<a name="tutorial-queues-spot-latency"></a>

If your game includes regional player latency information in game session placement requests, FleetIQ places players into game sessions with the lowest possible latency\. For game session requests that include multiple players, FleetIQ uses the average latency of all players in the request\. While this approach works most of the time, it can put players into a game session with unacceptable latency\. 

To handle this, you can set up a player latency policy in your queue to enforce a latency limit\. This policy ensures that no player gets put into a game session with an unacceptable latency\. A latency policy can increase player wait times for game session placements, or it can prevent a placement altogether\. To prevent this, your policy can include expansion rules that increase the maximum allowed latency over time\. For more information about creating latency policies, see [Create a player latency policy](queues-design.md#queues-design-latency)\.

**Tip**  
To manage latency specific rules, such as requiring similar latency across all players in a group, you can use [GameLift FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/) to create latency\-based matchmaking rules\.

**Tutorial example**  
Our game includes latency information in game session placement requests, and we have a player party feature that creates a game session for a group of players\. We can have players wait a little longer to get into games with the best possible gameplay experience\. Our game tests show that latencies under 50 milliseconds are optimal, and the game becomes unplayable at latencies over 250 milliseconds\. Players' tolerance for wait times starts to fail at about one minute\. We can use this information to set the parameters of our player latency policy\.

For our queue, which has a 300 second timeout, we add policy statements limiting the allowable latency\. The policy statements gradually allow larger latency values up to the 250 millisecond latency described earlier\.

With this policy, our queue looks for placements with optimal latency \(under 50ms\) for the first minute, and then relaxes the limit fairly quickly after that\. The queue does not make any placements where one player would have a latency of 250ms or higher\. 

![\[Diagram shows the example queue from step four with player latency policies added. The player latency policies state, enforce 50ms limit for 60s, enforce 125ms limit for 30s, and enforce 250ms limit until timeout.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/tutorial-queue-spot-step5.png)

## Summary<a name="tutorial-queues-spot-summary"></a>

Congratulations\! You now have a game session queue scoped for a particular segment of your player population\. It uses Spot fleets effectively and is resilient when Spot interruptions occur\. In addition, your queue's fleets are strategically prioritized and you've added hard latency limits to protect players from bad gameplay experiences\.

You can now use the queue to place game sessions for the player segment it serves\. When making game session placement requests for these players, reference this game session queue name in the request\. For more information on making game session placement requests, see [Create game sessions](gamelift-sdk-client-api.md#gamelift-sdk-client-api-create) or [Integrating a game client for Realtime Servers](realtime-client.md)\.