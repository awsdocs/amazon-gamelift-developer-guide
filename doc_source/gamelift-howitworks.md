# How GameLift works<a name="gamelift-howitworks"></a>

This topic provides a general overview of the GameLift managed hosting solution\. It covers the core components for game hosting and describes how GameLift makes your multiplayer game servers available to players\. If you want to learn more about the mechanisms for GameLift managed hosting, start with this topic and the following related topics\. To learn about other GameLift solutions, see [What Is Amazon GameLift?](gamelift-intro.md)\. 
+ [How Players Connect to Games](game-sessions-intro.md)
+ [Game Engines and Amazon GameLift](integration-engines.md)
+ [Setting up GameLift queues for game session placement](queues-intro.md)
+ [How GameLift FlexMatch works](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/gamelift-match.html)

Ready to start prepping your game for hosting on GameLift? See these [Getting Started with Amazon GameLift ](getting-started-intro.md) topics, including integration pathways\.

## Key components<a name="gamelift-howitworks-components"></a>

Setting up GameLift to host your game involves working with the following components\. The relationships between these components is illustrated in [Game architecture with managed GameLift ](gamelift-architecture.md)\.
+ A **game server** is your game's server software running in the cloud\. You upload your **game server build** \(or **script** if you're using [Realtime Servers](realtime-howitworks.md)\) to the GameLift service and tell GameLift how many game servers make available for game sessions\.
+ A **game session** is your game in progress with players\. You define the basic characteristics of a game session, such as its life span and number of players\. Players connect to the game server to join a game session\. 
+ The **GameLift service** manages the computing resources to host your game server and makes it possible for players to connect to games\. It regulates the resources needed to host games, finds available game servers to host new game sessions, and puts players into games\. The service also collects metrics on hosting activity and server health\.
+ A **game client** is your game's software running on a player's device\. Players might use the game client to get information about available game sessions and/or request to join games\. Game clients connect directly to a game server to join a game session, based on connection information it receives from the GameLift service\. 
+ **Game services** are additional custom services that you might create to handle special tasks related to GameLift\. As a best practice, all client communication with the GameLift service is handled by a client service; this is recommended for security reasons and efficiency\.

For a detailed description of how these components interact, see [GameLift and game client/server interactions](gamelift-sdk-interactions.md)\. 

## Hosting game servers<a name="gamelift-howitworks-allocating"></a>

Your uploaded game servers are hosted on GameLift virtual computing resources, called **instances**\. You set up your hosting resources by creating a **fleet** of instances and deploying them to run your game servers \(either a custom game server or Realtime Servers\)\. You can design a fleet to fit your game's needs\.

**Fleet architecture**  
Build a fleet that suits your core requirements\. 
+ **What type of resources does your game need? ** – GameLift supports a range of operating systems and instance types\. An instance type is a specific configuration of computing hardware, including processing power, memory, and networking capacity\. Note that hosting costs are based on the instance type, location, and volume of instances you use\. Depending on your game's requirements, you might opt to use many small instances or fewer more powerful instances\. Learn more about how to [Choosing computing resources](gamelift-ec2-instances.md)\.
+ **Where do you want to run your game servers?** – You set up fleets to deploy instances wherever you have players waiting to join games\. You can build multi\-location fleets that deploy instances to any or all AWS **Regions** that are supported in GameLift\. See more information on [Using GameLift in AWS Regions](gamelift-regions.md)\.
+ **How critical is game server reliability?** – Your fleet uses either Spot instances or On\-Demand instances\. Spot instances \(based on EC2's Spot instances\) usually cost less, but may be interrupted during a game session\. However, GameLift has additional safeguards that make game session interruptions extremely rare, and fleets with Spot instances are a good choice for most games\. On\-demand instances, in contrast, provide consistent availability but can be more expensive\. They are a good option for games where players are strongly impacted if a game session is interrupted\. Learn more about [On\-Demand versus Spot Instances](gamelift-ec2-instances.md#gamelift-ec2-instances-spot)\.
+ **How many players do you need to support?** – A fleet can have many instances, each one capable of hosting multiple simultaneous game sessions \. You can add or remove instances from your fleet as needed, and you can use auto\-scaling to automatically adjust as player demand shifts\. Learn more about [Scaling fleet capacity](#gamelift-howitworks-capacity)\.

**Server runtime configuration**  
A fleet instance can run multiple processes simultaneously, and it can run any executable in your game server build\. To determine what processes each instance should run, you create a runtime configuration\. The configuration specifies: \(1\) which executables to run, \(2\) how many processes of each executable to run concurrently, and \(3\) any launch parameters to use when starting each executable\. The number of processes that an instance can run simultaneously depends on the instance's computing power \(instance type\) as well as the requirements of your game server build\. Learn more about [running multiple processes on a fleet](fleets-multiprocess.md)\. Runtime configurations can be updated throughout the life of the fleet\.

A runtime configuration can also affect how new game sessions are started on an instance\. Some games require a lot of resources during the start\-up phase, and it can be a good idea to limit the instance resources that are used to activate new game sessions at any one time\. You can specify a maximum number of simultaneous game session activations per instance\. You can also place a time limit on each game session activation to quickly detect and shut down any game sessions that are failing to activate\.

**Server security**  
Enable PKI resource generation for a fleet\. When this feature is turned on, GameLift generates a TLS certificate for the fleet and creates a DNS entry for each instance in the fleet\. With these resources, your game can authenticate the client and server connection and encrypt all game client/server communication\. This feature is particularly valuable when deploying mobile multi\-player games\. These services are provided through the AWS Certificate Manager \(ACM\) and are currently available at no additional cost\.

**Fleet aliases**  
An **alias** is a designation that can be transferred from one actual fleet to another, making it a convenient way to genericize a fleet location\. For example, your game client needs to specify where \(which fleet\) to place a new game session\. Using an alias lets you switch game clients from using one fleet to another without having to change your game client\. There are several GameLift features where you have the option to specify a fleet or an alias\. You can also create a "terminal" alias, which lets you point to content \(such as a URL\) instead of connecting to a server\. This capability can be useful, for example, to prompt players to upgrade their clients\. 

## Running game sessions<a name="gamelift-howitworks-placing"></a>

After your game server code is successfully deployed to a fleet and game server processes are launched on each instance, the fleet is ready to host game sessions\. New game sessions are started when your game client service sends a placement request to the GameLift service\. 

**Game session placement and FleetIQ**  
Game session placement handles the task of selecting an available game server to host a new game session\. The key component for game session placement is the GameLift **game session queue**\. Each placement request for a new game session is sent to a specified queue for processing\. 

A game session queue is assigned a list of fleets, which determines where it can place game sessions\. As a best practice, a queue's fleets vary in fleet type \(Spot or On\-Demand\), locations, and/or instance type \(computing hardware\)\. This diversity gives the queue greater flexibility to make placements wherever they most make sense for players\. Queues with Spot instances can be configured to minimize the impact of Spot instance interruptions on your game sessions\. See more about game session queues and how to design them for your game in [Design a game session queue](queues-design.md)\. 

The game session placement process does more than simply place new game sessions with any available game server it finds\. Queues use a special algorithm, called FleetIQ, to look for the "best possible" placement for each game session request\. For more information on FleetIQ, see [How GameLift FleetIQ works](queues-design-fleetiq.md)\. The FleetIQ algorithm prioritizes the search for available game servers based on low player latency, hosting costs, geographical locations, or other fleet characteristics\. 

You create a game session queue to serve each pool of players for your game\. Player pools are groups of people who can play together in the same game sessions\. For example, you might have different player pools for people who speak different languages or use different game versions\.

**Note**  
GameLift gives you the option to locate and place game sessions manually, a useful option if you want to build a custom game session placement mechanism\. However, this approach bypasses the placement optimizations offered by FleetIQ\. If you're using FlexMatch matchmaking or Spot fleets, game session queues are required\.

**Player connections to games**  
As part of the game session placement process, the queue prompts the selected game server to start a new game session\. The game server, which has been integrated with the GameLift Server SDK, responds to the prompt and reports back to the GameLift service when it's ready to accept player connections\. The GameLift service then delivers connection information to your game client service, in the form of an IP address or DNS name and a port\. Your game clients use this information to connect directly to the game session and begin gameplay\. 

If the fleet is created with a TLS certificate, your game client and server can use it to establish a secure connection\.

You can optionally set up your game server to verify incoming players with the GameLift service, and to regularly report player connection status\. These options are recommended to help GameLift keep track of game session usage\. 

## Scaling fleet capacity<a name="gamelift-howitworks-capacity"></a>

Once a fleet is active and ready to host game sessions, you can adjust your fleet capacity to meet player demand\. The cost of hosting is based on the amount of capacity you use, so you'll want to find a balance between making sure all incoming players can find a game and overspending on resources that sit idle\. 

You scale a fleet by adjusting the number of instances in it\. For fleets with instances in multiple locations, you adjust capacity by location instead of for the entire fleet\.

Your runtime configuration determines how many game sessions and players each instance can host, so by scaling instances you're increasing or decreasing the availability for game sessions and players\. GameLift provides a highly effective auto\-scaling tool, or you can opt to manually set fleet capacity\. Learn more about how to [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\. 

### Auto\-scaling<a name="gamelift-howitworks-autoscale"></a>

With auto\-scaling enabled, GameLift tracks a fleet's hosting metrics and determines when to add or remove instances based on a set of guidelines that you define\. With the right auto\-scaling policies in place, GameLift can adjust capacity directly in response to changes in player demand\. Learn more about [improving cost efficiency with automatic scaling](https://aws.amazon.com/gamelift/pricing)\. For multi\-location fleets, auto\-scaling policies apply to an entire fleet, but you have the option to turn them on or off for each location\.

There are two methods of auto\-scaling available: 
+ **Target\-based scaling** – With this method, you specify a desired outcome and GameLift scales the fleet up or down to achieve that outcome\. Target tracking uses the metric "percent available game sessions", that is, the percentage of healthy server processes that are not currently hosting a game session\. Available game sessions is your buffer\-\-they represent the number of new game sessions and new players that can join a game with a minimal wait time\. With target tracking, you choose the buffer size that fits your game\. For example, for a game with highly volatile demand, you may need a larger buffer size\. This method is the preferred option, as it is simpler and more effective for more games\. Learn more about [how Target Tracking works](https://aws.amazon.com/blogs/gametech/autoscaling-with-amazon-gamelift-target-tracking/)\.
+ **Rule\-based scaling** – This method gives your more fine\-grained control of scaling actions\. It is also more complex to set up and manage and is more likely to have unexpected results\. Each policy specifies when to trigger a scaling event and what action to take in response\. For example, a policy might state: "If idle instances falls below 20 for 10 consecutive minutes, then increase capacity by 10%\." Most fleets require multiple policies to manage fleet capacity effectively, but multiple policies can have unexpected compound effects, which adds to the complexity\. Learn how to [Auto\-scale with rule\-based policies](fleets-autoscaling-rule.md)\. 

### Capacity scaling in action<a name="gamelift-howitworks-scaling"></a>

A scaling event can be triggered in several ways, either by making a change to the desired capacity through auto\-scaling or manual scaling, or when instances are shut down for health or other reasons\. Essentially, all scaling events are triggered when a fleet or location's "desired" instance count does not match its "active" instance count\. This circumstance causes GameLift to add or remove instances, as needed, to make the active instance count match the desired instance count\.
+ When desired instance count exceeds active instance count, GameLift requests additional instances and, once available, begins the process of installing the game server build to the new instances and starting up the game server processes\. As soon as one server process is active on an instance, the number of active instances is increased by one\. GameLift continues to add instances until the two count values are even\.
+ When active instance count exceeds desired instance count, GameLift begins searching for instances it can remove\. Any available instance \(that is, one not hosting any game sessions\) can be terminated, as well as any non\-protected instance even when hosting game sessions\. If no instances can be removed, the scale\-down event fails\. In this circumstance, the disparity between desired and active instance counts will continue to trigger scale\-down events until an instance is free to be removed\. GameLift then starts the termination process, which includes notifying all server processes on the instance to initiate a graceful shutdown\. Once the instance is terminated, the number of active instances is decreased by one\. GameLift continues to remove instances until the active and desired instance count values are even\.

### Additional scaling features<a name="gamelift-howitworks-protection"></a>

Additional features related to fleet capacity and scaling include: 
+ **Game session protection** – Prevent game sessions that are hosting active players from being terminated during a scale\-down event\. Game session protection can be turned on fleet\-wide, or it can be turned on for individual game sessions\. An instance cannot be terminated if any of its server processes are hosting protected game sessions\. Game sessions are not protected from termination due to health or for spot\-instance\-related interruptions \(see [On\-Demand versus Spot Instances](gamelift-ec2-instances.md#gamelift-ec2-instances-spot)\)\.
+ **Scaling limits** – Control overall instance usage by setting minimum and maximum limits on the number of instances in a fleet\. These limits apply when auto\-scaling or when manually setting capacity\. 
+ **Enabling/disabling auto\-scaling** – Switch auto\-scaling on or off at the fleet level without changing or deleting your auto\-scaling policies\. This feature allows you to temporarily scale your fleets manually when needed\.
+ **Scaling metrics** – Track a fleet's history of capacity and scaling events in graph form\. View capacity in conjunction with fleet utilization metrics to evaluate the effectiveness of your scaling approach\. 

## Monitoring fleet activity and troubleshooting<a name="gamelift-howitworks-telemetry"></a>

Once you have fleets up and running, GameLift collects a variety of information to help you monitor the performance of your deployed game servers\. Use this information to optimize your use of resources, troubleshoot issues, and gain insight into how players are active in your games\.
+ **Fleet, location, game session, and player session details** – This data includes status, which can help identify health issues, as well as details such as game session length and player connection time\. 
+ **Utilization metrics** – GameLift tracks fleet metrics over time:
  + For instances: network activity and CPU utilization
  + For server processes: number of active processes, new activations, and terminations
  + For games and players: number of active game sessions and player sessions
+ **Server process health** – GameLift tracks the health of each server process running on a fleet, including the number of healthy processes, percent of active processes that are healthy, and number of abnormal terminations\. 
+ **Game session logs** – You can have your game servers log session data and set GameLift to collect and store the logs once the game session ends\. Logs can then be downloaded from the service\. 

All of this data is available through the [GameLift console](https://console.aws.amazon.com/gamelift/)\. The console dashboard presents an overview of activity across all you builds and fleets as well as the option to drill down to more detailed information\. 

## Interacting with other AWS resources<a name="gamelift-howitworks-vpc"></a>

In many situations, you want your hosted game servers and applications to be able to communicate with other AWS resources\. For example, you might use a set of web services for player authentication or social networking\. This type of communication poses a challenge due to resource ownership issues\. When you deploy game servers using Amazon GameLift, the fleets and instances are allocated to your account but they are owned and managed by the GameLift service\. As a result, to access AWS resources that are managed by your AWS account, you need to explicitly allow access by the Amazon GameLift service\.

GameLift provides a couple of options for managing this type of access\. Learn more about how to [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.