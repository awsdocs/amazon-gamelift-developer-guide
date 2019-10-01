# How Amazon GameLift Works<a name="gamelift-howitworks"></a>

This topic provides a general overview of Amazon GameLift components and how the service makes your multiplayer game servers available to players\. If you want to learn more about what GameLift does and how the key features work, start with this topic and these related topics:
+ [How Realtime Servers Work](realtime-howitworks.md)
+ [How Amazon GameLift FlexMatch Works](gamelift-match.md)
+ [How Players Connect to Games](game-sessions-intro.md)
+ [Game Engines and Amazon GameLift](integration-engines.md)
+ [Why Use Queues?](queues-design.md#queues-design-benefits)

Ready to start prepping your game for hosting on GameLift? See these [Getting Started with Amazon GameLift ](getting-started-intro.md) topics, including integration roadmaps for custom servers or for Realtime servers\.

## Key Components<a name="gamelift-howitworks-components"></a>

Setting up Amazon GameLift to host your game involves working with a set of key components\. The relationships between these components is illustrated in [Game Architecture with Amazon GameLift ](gamelift-architecture.md)\.
+ A **game server** is your game's server software running in the cloud\. You might have a fully custom **game server build** containing the server executables, supporting assets, libraries, and dependencies\. Or, if you're using Realtime Servers, you have a **configuration script** with some optional custom game logic\. You upload your game server build or script to the Amazon GameLift service, and GameLift deploys it to to virtual computing resources for hosting\. 
+ A **game session** is a instance of your game server, running on Amazon GameLift resources, that players connect to and interact with\. You define the basic characteristics of a game session, such as its life span and number of players\. 
+ The **Amazon GameLift service** manages the computing resources to host your game server and makes it possible for players to connect to games\. It regulates the resources needed to meet player demand, starts new game sessions, and handles new player requests by finding and reserving player slots in active game sessions\. The service also collects metrics on player usage and server health\.
+ A **game client** is your game's software running on a player's device\. A game client makes requests to GameLift service about available game sessions\. It also connects directly to a game session using information that it receives from the GameLift service\. 
+ **Game services** are additional custom services that you might create to handle special tasks related to GameLift\. For example, most games use a client service to handle handle communication with the GameLift service, instead of having game clients call the service directly\. 

For a detailed description of how these components interact, see [Amazon GameLift and Game Client/Server Interactions](gamelift-sdk-interactions.md)\. 

## Hosting Game Servers<a name="gamelift-howitworks-allocating"></a>

Your uploaded game servers are hosted on Amazon GameLift virtual computing resources, called **instances**\. You set up your hosting resources by creating a **fleet** of instances and deploying them to run your game server \(either your custom game server or your configured Realtime Servers\)\. You can design a fleet to fit your game's needs\.

**Fleet architecture**  
Build a fleet that suits your core requirements\. 
+ **What type of resources does your game need? ** – GameLift supports a range of operating systems and instance types\. The instance type determines the kind of computing hardware used, including processing power, memory, and networking capacity\. Note that fleet costs are based on both the type and the number of instances you use\. Depending on your game's requirements, you might opt to use many small instances or fewer more powerful instances\. Learn more about how to [Choose Computing Resources](gamelift-ec2-instances.md)\.
+ **Where do you want to run your game servers?** – You set up fleets wherever you have players waiting to join your games\. Each fleet is deployed to a single AWS **region**, but you can create fleets in as many regions as you need\. See the available regions for Amazon GameLift at [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#gamelift-region)\.
+ **How critical is game server reliability?** – Your fleet uses either Spot instances or On\-Demand instances\. Spot instances \(based on EC2's Spot instances\) usually cost less, but may be interrupted during a game session\. However, GameLift has additional safeguards that make game session interruptions extremely rare, and fleets with Spot instances are a good choice for most games\. On\-demand instances, in contrast, provide consistent availability but can be more expensive\. They are a good option for games where players are strongly impacted if a game session is interrupted\. Learn more about Spot and On\-Demand instances
+ **How many players do you need to support?** – A fleet can have many instances, each one capable of hosting multiple simultaneous game sessions \. You can add or remove instances from your fleet as needed, and you can use auto\-scaling to automatically adjust as player demand shifts\. Learn more about [Scaling Fleet Capacity](#gamelift-howitworks-capacity)\.

**Server runtime configuration**  
A fleet instance can run multiple processes simultaneously, and it can run any executable in your game server build\. To determine how processes should run on each instance, you create a runtime configuration\. The configuration specifies: \(1\) which executables to run, \(2\) how many processes of each executable to run concurrently, and \(3\) any launch parameters to use when starting each executable\. The number of processes that an instance can run simultaneously depends on the instance's computing power \(instance type\) as well as the requirements of your game server build\. Learn more about [running multiple processes on a fleet](fleets-multiprocess.md)\. Runtime configurations can be updated throughout the life of the fleet\.

A runtime configuration can also affect how new game sessions are started on an instance\. Some games require a lot of resources during the start\-up phase, and it can be a good idea to limit the instance resources that are used to activate new game sessions at any one time\. You can specify a maximum number of simultaneous game session activations per instance\. You can also place a time limit on each game session activation to quickly detect and shut down any game sessions that are failing to activate\.

**Server security**  
Enable PKI resource generation for a fleet\. When this feature is turned on, GameLift generates a TLS certificate for the fleet and creates a DNS entry for each instance in the fleet\. With these resources, your game can authenticate the client and server connection and encrypt all game client/server communication\. This feature is particularly valuable when deploying mobile multi\-player games\. These services are provided through the AWS Certificate Manager \(ACM\) and are currently available at no additional cost\.

**Fleet aliases**  
An **alias** is a designation that can be transferred from one actual fleet to another, making it a convenient way to genericize a fleet location\. For example, your game client needs to specify where \(which fleet\) to place a new game session\. Using an alias lets you switch game clients from using one fleet to another without having to change your game client\. There are several GameLift features where you have the option to specify a fleet or an alias\. You can also create a "terminal" alias, which lets you point to content \(such as a URL\) instead of connecting to a server\. This capability can be useful, for example, to prompt players to upgrade their clients\. 

## Running Game Sessions<a name="gamelift-howitworks-placing"></a>

Once a game server build is successfully deployed to a fleet, the fleet is ready to host game sessions\. To start a new game session for one or more players, your game client sends a request \(via a game service\) to the GameLift service\. When the request is received, GameLift uses a feature called FleetIQ to place the new game session with the "best possible" fleet\.

The meaning of "best possible fleet" is defined by you, based on your game priorities, when you define a game session **queue**\. A queue creates a group of one or more fleets and defines how to choose the best fleet in the group for a new game session\. Queues can contain fleets that are located in different regions\. A new game session request specifies which queue to use, and GameLift may place the new game session with any available fleet in the queue\. For example, you might use a queue with fleets in each of the five North American regions\. With this type of multi\-region queue, GameLift is more likely to find available resources, even during heavy traffic, and start up a new game session quickly\. Fleet availability simply means that somewhere in the fleet there is at least one instance with a game server process that is free to host a new game session\.

When choosing the best possible placement for a new game session, FleetIQ uses one of two methods: 
+ **Evaluate player latency** – Requests for new game sessions can include ping time for each player in the request\. If this data is provided, FleetIQ evaluates it to determine which regions will deliver the lowest possible latency for the players\. GameLift uses this information to place the new game session\. You can also define latency policies to ensure that players are never placed in game sessions with unacceptable latency\. 
+ **Use prioritized fleet list** – For requests that don't contain player latency data, FleetIQ places new game sessions based on the order that the fleets are listed in the queue\. You can prioritize fleets in a queue by placing them at the top of the fleet list order\. This method usually results in all game sessions being placed with the first fleet listed, and the remaining fleets serve as backups when the first fleet is full\. 

The queue is a powerful concept that can be used to solve a range of capacity and availability issues\. Queues can be used to balance resource usage across regions, reduce the amount of wait time for players during unexpected spikes in demand, and mitigate region slowdowns or outages\. You can also use them to create player pools that span regions, so that players in different regions can play together\. Queues are required when using FlexMatch matchmaking or GameLift Spot fleets\. Learn more about how to [Design a Game Session Queue](queues-design.md)\.

Once a game session is started on a fleet instance, the GameLift service delivers connection information to the game client, in the form of an IP address or DNS name and a port\. Your game client uses this information to connect to your game server\. Depending on how you set up your game server, on connection, it may communicate with the GameLift service to verify the player and report player connection status\.

If the fleet is created with a TLS certificate, your game client and server can use it to establish a secure connection\.

## Scaling Fleet Capacity<a name="gamelift-howitworks-capacity"></a>

Once a fleet is active and ready to host game sessions, you can adjust your fleet capacity to meet player demand\. The cost of hosting is based on the amount of capacity you use, so you'll want to find a balance between making sure all incoming players can find a game and overspending on resources that sit idle\. 

You scale a fleet by adjusting the number of instances in it\. Your runtime configuration determines how many game sessions and players each instance can host, so by scaling instances you're increasing or decreasing the availability for game sessions and players\. GameLift provides a highly effective auto\-scaling tool, or you can opt to manually set fleet capacity\. Learn more about how to [Scaling Amazon GameLift Fleet Capacity](fleets-manage-capacity.md)\. 

### Auto\-scaling<a name="gamelift-howitworks-autoscale"></a>

With auto\-scaling enabled, GameLift tracks a fleet's hosting metrics and determines when to add or remove instances based on a set of guidelines that you define\. With the right auto\-scaling policies in place, GameLift can adjust capacity directly in response to changes in player demand\. Learn more about [improving cost efficiency with automatic scaling](https://aws.amazon.com/gamelift/pricing)\.

There are two methods of auto\-scaling available: 
+ **Target\-based scaling** – With this method, you specify a desired outcome and GameLift scales the fleet up or down to achieve that outcome\. Target tracking uses the metric "percent available game sessions", that is, the percentage of healthy server processes that are not currently hosting a game session\. Available game sessions is your buffer\-\-they represent the number of new game sessions and new players that can join a game with a minimal wait time\. With target tracking, you choose the buffer size that fits your game\. For example, for a game with highly volatile demand, you may need a larger buffer size\. This method is the preferred option, as it is simpler and more effective for more games\. Learn more about [how Target Tracking works](https://aws.amazon.com/blogs/gametech/autoscaling-with-amazon-gamelift-target-tracking/)\.
+ **Rule\-based scaling** – This method gives your more fine\-grained control of scaling actions\. It is also more complex to set up and manage and is more likely to have unexpected results\. Each policy specifies when to trigger a scaling event and what action to take in response\. For example, a policy might state: "If idle instances falls below 20 for 10 consecutive minutes, then increase capacity by 10%\." Most fleets require multiple policies to manage fleet capacity effectively, but multiple policies can have unexpected compound effects, which adds to the complexity\. Learn how to [Auto\-Scale with Rule\-Based Policies](fleets-autoscaling-rule.md)\. 

### Fleet scaling in action<a name="gamelift-howitworks-scaling"></a>

A fleet scaling event can be triggered in several ways, either by making a change to the desired capacity through auto\-scaling or manual scaling, or when instances are shut down for health or other reasons\. Essentially, all scaling events are triggered when a fleet's "desired" instance count does not match its "active" instance count\. This circumstance causes Amazon GameLift to add or remove instances, as needed, in order to make the active instance count match the desired instance count\.
+ When desired instance count exceeds active instance count, Amazon GameLift requests additional instances and, once available, begins the process of installing the game server build to the new instances and starting up the game server processes\. As soon as one server process is active on an instance, the number of active instances is increased by one\. Amazon GameLift continues to add instances until the two count values are even\.
+ When active instance count exceeds desired instance count, Amazon GameLift begins searching for instances it can remove\. Any available instance \(that is, not hosting any game sessions\) can be terminated, as well as any non\-protected instance even when hosting active game sessions\. If no instances can be removed, the scale\-down event fails\. In this circumstance, the disparity between desired and active instance counts will continue to trigger scale\-down events until an instance is free to be removed\. Amazon GameLift then starts the termination process, which includes notifying all server processes on the instance to initiate a graceful shutdown\. Once the instance is terminated, the number of active instances is decreased by one\. Amazon GameLift continues to remove instances until the two count values are even\.

### Additional scaling features<a name="gamelift-howitworks-protection"></a>

Additional features related to fleet capacity and scaling include: 
+ **Game session protection** – Prevent game sessions that are hosting active players from being terminated during a scale\-down event\. Game session protection can be turned on fleet\-wide, or it can be turned on for individual game sessions\. An instance cannot be terminated if any of its server processes are hosting protected game sessions\. Game sessions are not protected from termination due to health or for spot\-instance\-related interruptions \(see [On\-Demand versus Spot Instances](gamelift-ec2-instances.md#gamelift-ec2-instances-spot)\)\.
+ **Scaling limits** – Control overall instance usage by setting minimum and maximum limits on the number of instances in a fleet\. These limits apply when auto\-scaling or when manually setting capacity\. 
+ **Enabling/disabling auto\-scaling** – Switch auto\-scaling on or off at the fleet level without changing or deleting your auto\-scaling policies\. This feature allows you to temporarily scale your fleets manually when needed\.
+ **Scaling metrics** – Track a fleet's history of capacity and scaling events in graph form\. View capacity in conjunction with fleet utilization metrics to evaluate the effectiveness of your scaling approach\. The following graph shows a fleet with target tracking set to a 15% buffer; the percentage of available game session slots \(in green\) automatically adjusts as fleet capacity \(in blue and orange\) changes\. 

## Monitoring Fleet Activity and Troubleshooting<a name="gamelift-howitworks-telemetry"></a>

Once you have fleets up and running, Amazon GameLift collects a variety of information to help you monitor the performance of your deployed game servers\. Use this information to optimize your use of resources, troubleshoot issues, and gain insight into how players are active in your games\.
+ **Fleet, game session, and player session details** – This data includes status, which can help identify health issues, as well as details such as game session length and player connection time\. 
+ **Utilization metrics** – Amazon GameLift tracks fleet metrics over time:
  + For instances: network activity and CPU utilization
  + For server processes: number of active processes, new activations, and terminations
  + For games and players: number of active game sessions and player sessions
+ **Server process health** – Amazon GameLift tracks the health of each server process running on a fleet, including the number of healthy processes, percent of active processes that are healthy, and number of abnormal terminations\. 
+ **Game session logs** – You can have your game servers log session data and set Amazon GameLift to collect and store the logs once the game session ends\. Logs can then be downloaded from the service\. 

All of this data is available through the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/)\. The console dashboard presents an overview of activity across all you builds and fleets as well as the option to drill down to more detailed information\. 

## Networking With AWS Resources<a name="gamelift-howitworks-vpc"></a>

In many situations, you want your hosted game servers and applications to be able to communicate with your other AWS resources\. For example, you might use a set of web services to support your game, such as for player authentication or social networking\. This type of communication poses a challenge due to ownership issues\. When you deploy game servers using Amazon GameLift, the fleets and instances are allocated to your account but they are owned and managed by the Amazon GameLift service\. As a result, to access AWS resources that are managed by your AWS account, you need to explicitly permit access by the Amazon GameLift service\.

Amazon GameLift provides a couple of options for managing this type of access\. Learn more about how to [Access AWS Resources From Your Fleets](gamelift-sdk-server-resources.md)\.