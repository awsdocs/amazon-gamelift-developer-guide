# How Amazon GameLift Works<a name="gamelift-howitworks"></a>

This topic provides an overview of Amazon GameLift components and how the service works to deploy your multiplayer game servers and manage player traffic\.

## Key Components<a name="gamelift-howitworks-components"></a>

Setting up Amazon GameLift to host your game involves working with the following components:
+ A **game server** is your game's server software running in the cloud\. You upload a **game server build** to Amazon GameLift, including the server executables, supporting assets, libraries, and dependencies\. Amazon GameLift deploys your game server to virtual computing resources for hosting\.
+ A **game session** is a instance of your game server, running on Amazon GameLift, that players connect to and interact with\. A game defines the basic characteristics of a game session, such as its life span or number of players\. 
+ The **Amazon GameLift service** manages the computing resources needed to host your game server and makes it possible for players to connect to games\. It regulates the number of resources for player demand, starts and stops game sessions, and handles player join requests by finding and reserving player slots in active game sessions\. The service also collects performance data on server process health and player usage\.
+ A **game client** is your game's software running on a player's device\. Using the game client, a player can connect to a game session that is being hosted on Amazon GameLift\. 
+ **Game services** might communicate with the Amazon GameLift service for a variety of purposes\. For example, you might create a game service to act as an intermediary between game clients and servers, such as to manage matchmaking or player authentication\.

See [Amazon GameLift and Game Client/Server Interactions](gamelift-sdk-interactions.md) for a detailed description of how these components interact\.

## Hosting Game Servers<a name="gamelift-howitworks-allocating"></a>

To host a game server on Amazon GameLift, you need a **fleet** of virtual computing resources, called **instances**\. The instance type you choose for a fleet determines the computing hardware \(including power, memory, networking capacity, etc\.\) that will be used to host your game servers\. Learn more about how to [Choose Computing Resources](gamelift-ec2-instances.md), including how to work with spot instances to reduce hosting costs\. 

Each instance in a fleet can run multiple** server processes** simultaneously, depending on the hardware capability, and each server process can host one game session\. Since a game server build can have one or multiple executable files, you can configure a fleet to run multiple server processes of each executable on each instance\. You'll need to find the right configuration to balance the computing requirements of your game server\-\-and the number of server processes to run\-\-against the capabilities of the instance type you choose\. Learn more about [running multiple processes on a fleet](fleets-multiprocess.md)\.

A fleet deploys your game server build to a single AWS **region**\. For each region where you want to deploy your game server build, you must set up a separate fleet\. See a list of available regions for Amazon GameLift at [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#gamelift-region)\.

You may want to assign an **alias** to a fleet\. An alias is a convenient way to genericize how game clients connect to game servers\. Because an alias can be changed to point to any fleet you want, referencing an alias ID in your client instead of a fleet ID helps you gracefully transition players from one fleet to another—without having to deploy game client updates\. 

## Placing Game Sessions<a name="gamelift-howitworks-placing"></a>

Once a game server build is successfully deployed to a fleet, the fleet is ready to host game sessions\. A game client or client service sends a request, on behalf of one or more players, for a new game session\. When a request for a new game session is received, Amazon GameLift uses the FleetIQ feature to search available hosting resources and place the new game session with the best possible fleet\.

You define what "best possible fleet" means for your game by creating a game session **queue**\. A queue identifies a list of fleets where new game sessions can be placed and defines how to choose the best fleet for each new game session\. When your game client or client service requests a new game session, it specifies which queue to use for placement\. The new game session can be placed with any fleet in the queue that has an available server process\. Queues can contain fleets that are located in different regions\. For example, you might set up a queue to place game sessions in any of the five North American regions\. With a multi\-region queue, Amazon GameLift is more likely to find available resources, even during heavy traffic, and start up a new game session quickly\. 

There are two ways that FleetIQ locates the best possible placement for a new game session\. The most effective way is by evaluating player latency\. Placement requests may include players' latency data for each region covered in the queue\. If latency data is provided, FleetIQ searches for hosting availability in the region that offers the lowest latency for the request's players\. If you opt not to provide latency data, you can manually prioritize a queue's fleet list\. When searching for hosting availability, Amazon GameLift starts at the top of the list and works down\. In this scenario, game sessions are usually placed with the top\-priority fleet, with additional fleets acting as backup\.

The queue is a powerful concept that can be used to solve a range of issues\. Queues can balance resource usage across regions and reduce the amount of wait time for players during unexpected spikes in demand\. They can be used to mitigate region slowdowns or outages\. You can also use them to create player pools that span regions\. Queues are required when using FlexMatch matchmaking or Amazon GameLift spot fleets\. Learn more about how to [Design a Game Session Queue](queues-design.md)\.

## Managing Capacity and Scaling<a name="gamelift-howitworks-capacity"></a>

Once a fleet is active and ready to host game sessions, you'll need to adjust fleet capacity to meet player demand\. Since the cost of hosting is based on the amount of capacity you use, it is important to find a balance between maintaining enough fleet capacity for incoming players and overspending for resources that sit unused during idle times\. 

Fleet capacity is measured in instances\. A fleet's configuration determines how many game sessions and players each instance can host\. To adjust fleet capacity, you increase or decrease the number of instances in the fleet\. Amazon GameLift provides a range of options for managing fleet capacity\. The most effective method, by far, is to use the Amazon GameLift auto\-scaling feature, but you can also set fleet capacity manually\. Learn more about how to [Scaling Amazon GameLift Fleet Capacity](fleets-manage-capacity.md)\. 

### Auto\-scaling<a name="gamelift-howitworks-autoscale"></a>

Auto\-scaling is a fast, efficient, and accurate way to match fleet capacity to player usage\. With auto\-scaling, Amazon GameLift tracks the fleet's hosting metrics and determines when to add or remove instances based on a set of guidelines, called **policies**, that you define\. With the right auto\-scaling policies in place, Amazon GameLift can adjust capacity directly in response to changes in player demand, so that the fleet always has room for new players without maintaining an excessive amount of idle resources\. Learn more about [improving cost efficiency with automatic scaling](https://aws.amazon.com/gamelift/pricing)\.

There are two types of auto\-scaling available, target\-based and rule\-based\. The recommended option is to use target\-based auto\-scaling as the simplest and most effective option\. Rule\-based auto\-scaling provides more fine\-grained control over scaling actions, but is difficult to set up and manage\. For most games, target tracking is sufficient\.

Target\-based auto\-scaling allows you to select a desired outcome and have Amazon GameLift scale the fleet up or down to achieve that outcome\. The Target Tracking tool is based on a single key metric—the percentage of resources that are available to host game sessions but are currently unused\. These resources are your buffer\-\-since they are ready to host games, they represent the number of new game sessions and new players that can join you game with minimal waiting\. Target tracking lets you choose a buffer size, as a percentage of total fleet capacity, that best fits your game\. For example, if demand for your game is highly volatile, you may want to use a larger buffer size\. With target tracking on, Amazon GameLift adds and removes instances as needed to maintain the target buffer size\. For most games, target tracking represents the best option for managing fleet capacity\. Learn how to [Auto\-Scale with Target Tracking](fleets-autoscaling-target.md)\. Learn more about [how Target Tracking works](https://aws.amazon.com/blogs/gametech/autoscaling-with-amazon-gamelift-target-tracking/)\. 

Rule\-based auto\-scaling provides more fine\-grained control over auto\-scaling activity, but it is significantly more complicated to manage\. Each rule\-based policy specifies when and how to trigger a scaling event\. For example, this policy statement might be used to trigger a scale\-up event: "If idle instances falls below 20 for 10 consecutive minutes, then increase capacity by 10%\." With rule\-based auto\-scaling, most fleets require multiple policies to automatically maintain fleet capacity\. Since policies are evaluated independently and can have unexpected compound effects, this option adds greater complexity to game hosting\. Learn how to [Auto\-Scale with Rule\-Based Policies](fleets-autoscaling-rule.md)\.

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