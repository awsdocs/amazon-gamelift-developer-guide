# How GameLift works<a name="gamelift-howitworks"></a>

This topic provides a general overview of the GameLift managed hosting solution\. It covers the core components for game hosting and describes how GameLift makes your multiplayer game servers available to players\. To learn more about GameLift managed hosting, start with this topic and the following related topics\.
+ [How players connect to games](game-sessions-intro.md)
+ [Game engines and Amazon GameLift](integration-engines.md)
+ [Setting up GameLift queues for game session placement](queues-intro.md)
+ [How GameLift FlexMatch works](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/gamelift-match.html)

Ready to start prepping your game for hosting on GameLift? Check out [Getting started with Amazon GameLift ](getting-started-intro.md)\.

## Key components<a name="gamelift-howitworks-components"></a>

Setting up GameLift to host your game involves working with the following components\. The diagram on [Game architecture with managed GameLift ](gamelift-architecture.md) visualizes the relationships between these components\.
+ A **game server** is your game's server software running in the cloud\. You upload your **game server build** \(or **script** if you're using [Realtime Servers](realtime-howitworks.md)\) to the GameLift service and tell GameLift the number of game servers to make available for game sessions\.
+ A **game session** is your game in progress with players\. You define the basic characteristics of a game session, such as its life span and number of players\. Players connect to the game server to join a game session\. 
+ The **GameLift service** manages the computing resources to host your game server and makes it possible for players to connect to games\. It regulates the resources needed to host games, finds available game servers to host new game sessions, and puts players into games\. The service also collects metrics on hosting activity and server health\.
+ A **game client** is your game's software running on a player's device\. Players might use the game client to get information about available game sessions and request to join games\. Game clients connect to a game server through backend services to join a game session, based on connection information it receives from the GameLift service\. 
+ **Backend services** are additional custom services that handle tasks related to GameLift\. As a best practice, a backend service handles all game client communication with the GameLift service\. Use backend services for enhanced security and efficiency\.

## Hosting game servers<a name="gamelift-howitworks-allocating"></a>

Your uploaded game servers are hosted on GameLift virtual computing resources, called **instances**\. You set up your hosting resources by creating a **fleet** of instances and deploying them to run your game servers\. You can design a fleet to fit your game's needs\.

**Fleet architecture**  
Build a fleet that suits your core requirements\. 
+ **What type of resources does your game need? ** – GameLift supports a range of operating systems and instance types\. An instance type is a specific configuration of computing hardware, including processing power, memory, and networking capacity\. The instance type, location, and volume of instances you use determines hosting cost\. Depending on your game's requirements, you might opt to use many small instances or fewer more powerful instances\. Learn more about how to [Choosing GameLift computing resources](gamelift-ec2-instances.md)\.
+ **Where do you want to run your game servers?** – You set up fleets to deploy instances wherever you have players waiting to join games\. You can build multi\-location fleets that deploy instances to any or all AWS locations that GameLift supports\. See more information on [GameLift hosting in AWS Regions and Local Zones](gamelift-regions.md)\.
+ **How critical is game server reliability?** – Your fleet uses either Spot instances or On\-Demand instances\. Spot instances cost less on average than On\-Demand instances, but may be interrupted during a game session\. GameLift has additional safeguards that make game session interruptions rare, and fleets with Spot instances are a good choice for most games\. On\-demand instances provide consistent availability but can be more expensive\. They're a good option for games where players are strongly impacted if a game session is interrupted\. Learn more about [On\-Demand Instances versus Spot Instances](gamelift-ec2-instances.md#gamelift-ec2-instances-spot)\.
+ **How many players do you need to support?** – A fleet can have many instances, each one capable of hosting multiple simultaneous game sessions \. You can add or remove instances from your fleet as needed, and you can use auto scaling to automatically adjust as player demand shifts\. Learn more about [Scaling fleet capacity](#gamelift-howitworks-capacity)\.

**Server runtime configuration**  
A fleet instance can run multiple processes simultaneously, and it can run any executable in your game server build\. To determine what processes each instance should run, you create a runtime configuration\. The configuration specifies: which executables to run, how many processes of each executable to run concurrently, and any launch parameters to use when starting each executable\. The number of processes that an instance can run simultaneously depends on the instance's computing power \(instance type\) as well as the requirements of your game server build\. Learn more about [running multiple processes on a fleet](fleets-multiprocess.md)\. 

A runtime configuration can also affect how GameLift starts new game sessions on an instance\. Some games require a lot of resources during the start\-up phase, and it can be a good idea to limit the instance resources that are used to activate new game sessions at any one time\. You can specify a maximum number of simultaneous game session activations per instance\. You can also place a time limit on each game session activation to quickly detect and shut down any game sessions that are failing to activate\.

**Server security**  
Enable PKI resource generation for a fleet\. When this feature is turned on, GameLift generates a TLS certificate for the fleet and creates a DNS entry for each instance in the fleet\. With these resources, your game can authenticate the client and server connection and encrypt all game client/server communication\. This feature is particularly valuable when deploying mobile multi\-player games\. AWS Certificate Manager \(ACM\) provides these services at no additional cost\.

**Fleet aliases**  
An **alias** is a designation that you can transfer between fleets, making it a convenient way to have a generic fleet location\. Using an alias lets you switch game clients from using one fleet to another without having to change your game client\. You can also create a terminal alias, which lets you point to content \(such as a URL\) instead of connecting to a server\. This capability can be useful, for example, to prompt players to upgrade their clients\. 

## Running game sessions<a name="gamelift-howitworks-placing"></a>

After you deploy your game server code to a fleet and GameLift launches game server processes on each instance, the fleet is ready to host game sessions\. GameLift starts new game sessions when your game client service sends a placement request to the backend service or GameLift service\. 

**Game session placement and FleetIQ**  
Game session placement handles the task of selecting an available game server to host a new game session\. The key component for game session placement is the GameLift **game session queue**\. 

You assign a game session queue a list of fleets, which determines where the queue can place game sessions\. As a best practice, a queue's fleets vary in fleet type \(Spot or On\-Demand\), locations, and instance type\. This diversity gives the queue greater flexibility to make placements wherever they most make sense for players\. You can configure queues with Spot instances to minimize the impact of Spot instance interruptions on your game sessions\. See more about game session queues and how to design them for your game in [Design a game session queue](queues-design.md)\. 

You can create a game session queue to serve each pool of players for your game\. Player pools are groups of people who can play together in the same game sessions\. For example, you might have different player pools for people who speak different languages or use different game versions\.

Queues use a special algorithm, called FleetIQ, to look for the best possible placement for each game session request\. The FleetIQ algorithm prioritizes the search for available game servers based on low player latency, hosting costs, geographical locations, or other fleet characteristics\. For more information on FleetIQ, see [How GameLift queues works](queues-intro.md#queues-design-fleetiq)\. 

**Note**  
GameLift gives you the option to locate and place game sessions manually, a useful option if you want to build a custom game session placement mechanism\. However, this approach bypasses the placement optimizations offered by FleetIQ\. FlexMatch matchmaking or Spot fleets require game session queues\.

**Player connections to games**  
As part of the game session placement process, the queue prompts the selected game server to start a new game session\. The game server, which has been integrated with the GameLift Server SDK, responds to the prompt and reports back to the GameLift service when it's ready to accept player connections\. The GameLift service then delivers connection information to your backend service or game client service, in the form of an IP address or DNS name and a port\. Your game clients use this information to connect directly to the game session and begin gameplay\. 

If you created the fleet with a TLS certificate, your game client and server can use it to establish a secure connection\.

You can optionally set up your game server to verify incoming players with the GameLift service, and to regularly report player connection status\. These options are recommended to help GameLift keep track of game session usage\. 

## Scaling fleet capacity<a name="gamelift-howitworks-capacity"></a>

When a fleet is active and ready to host game sessions, you can adjust your fleet capacity to meet player demand\. The amount of capacity you use determines the cost of hosting, so you want to find a balance between making sure all incoming players can find a game and overspending on resources that sit idle\.

You scale a fleet by adjusting the number of instances in it\. By scaling instances you're increasing or decreasing the availability for game sessions and players\. For fleets with instances in multiple locations, you adjust capacity by location instead of for the entire fleet\.

GameLift provides a highly effective auto scaling tool, or you can opt to manually set fleet capacity\. For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\.

### Auto scaling<a name="gamelift-howitworks-autoscale"></a>

With auto scaling enabled, GameLift tracks a fleet's hosting metrics and determines when to add or remove instances based on a set of guidelines that you define\. GameLift can adjust capacity directly in response to changes in player demand\. For information about improving cost efficiency with auto scaling, see [Amazon GameLift Pricing](http://aws.amazon.com/gamelift/pricing/)\. For multi\-location fleets, auto scaling policies apply to an entire fleet, but you have the option to turn them on or off for each location\.

There are two methods of auto scaling available: 
+ **Target\-based scaling** – Target\-based scaling \(also known as "target tracking"\) uses the metric `PercentAvailableGameSessions`, the percentage of server processes that aren't currently hosting a game session\. Available game sessions are your buffer, representing the number of new game sessions and new players that can join a game with minimal wait time\. With target\-based scaling, you choose the buffer size that fits your game\. For more information, see [Target\-based auto scaling](fleets-autoscaling-target.md)\.
+ **Rule\-based scaling** – This method gives you fine\-grained control of scaling actions\. Each rule\-based scaling policy specifies when to start a scaling event and what action to take in response\. For example, a policy might state: "If idle instances falls below 20 for 10 consecutive minutes, then increase capacity by 10%\." For more information, see [Auto scale with rule\-based policies](fleets-autoscaling-rule.md)\.

### Capacity scaling in action<a name="gamelift-howitworks-scaling"></a>

Scaling events occur when a fleet or location's desired instance count doesn't match its active instance count\. This causes GameLift to add or remove instances as needed to make the active instance count match the desired instance count\.
+ When the desired instance count exceeds the active instance count, GameLift requests additional instances\. When these desired instances are available, GameLift begins the process of installing the game server build to the new instances and starting up the game server processes\. GameLift continues to add instances until the active instance and desired count values are equal\.
+ When the active instance count exceeds the desired instance count, GameLift searches for instances that it can remove\. GameLift can terminate any instance not hosting game sessions, or any non\-protected instance even when hosting game sessions\. If GameLift can't remove instances, the scale\-down event fails\. New scale\-down events continue to occur until GameLift can remove an instance\. GameLift continues to remove instances until the active and desired instance count values are even\.

### Additional scaling features<a name="gamelift-howitworks-protection"></a>

Additional features related to fleet capacity and scaling include:
+ **Game session protection** – Prevent game sessions that are hosting active players from being terminated during a scale\-down event\. You can turn on fleet\-wide game session protection, or you can turn it on for individual game sessions\. Game sessions aren't protected from termination due to health or for Spot Instance\-related interruptions\.
+ **Scaling limits** – Control overall instance usage by setting minimum and maximum limits on the number of instances in a fleet\. These limits apply when auto scaling or when manually setting capacity\.
+ **Suspending auto scaling** – Suspend auto scaling at the fleet location\-level without changing or deleting your auto scaling policies\. You can use this feature to temporarily scale your fleets manually when needed\.
+ **Scaling metrics** – Track a fleet's history of capacity and scaling events in graph form\. View capacity in conjunction with fleet utilization metrics to evaluate the effectiveness of your scaling approach\. 

## Monitoring GameLift<a name="gamelift-howitworks-telemetry"></a>

Once you have fleets up and running, GameLift collects a variety of information to help you monitor the performance of your deployed game servers\. Use this information to optimize your use of resources, troubleshoot issues, and gain insight into how players are active in your games\.
+ **Fleet, location, game session, and player session details** – This data includes status, which can help identify health issues, and details such as game session length and player connection time\. 
+ **Utilization metrics** – GameLift tracks fleet metrics over time:
  + For instances: network activity and CPU utilization
  + For server processes: number of active processes, new activations, and terminations
  + For games and players: number of active game sessions and player sessions
+ **Server process health** – GameLift tracks the health of each server process running on a fleet, including the number of healthy processes, percent of active processes that are healthy, and number of abnormal terminations\. 
+ **Game session logs** – You can have your game servers log session data and set GameLift to collect and store the logs once the game session ends\. Logs can then be downloaded from the service\. 

For more information about monitoring in GameLift, see [Monitoring Amazon GameLift](monitoring-overview.md)\.

## Using other AWS resources<a name="gamelift-howitworks-vpc"></a>

In many situations, you want your hosted game servers and applications to be able to communicate with other AWS resources\. For example, you might use a set of web services for player authentication or social networking\. When you deploy game servers using Amazon GameLift, GameLift owns and manages the fleets and instances\. As a result, for your game servers to access AWS resources that your AWS account manages, explicitly allow access by the GameLift service to your AWS resources\.

GameLift provides a couple of options for managing this type of access\. Learn more about how to [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.