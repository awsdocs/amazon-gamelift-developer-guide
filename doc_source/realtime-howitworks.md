# How Realtime Servers Work<a name="realtime-howitworks"></a>

This topic provides an overview of the Amazon GameLift Realtime Servers feature, discusses when it is a good fit for your game, and explains how Realtime Servers supports multiplayer gaming\.

## Choosing Realtime Servers for Your Game<a name="realtime-howitworks-gametype"></a>

While Amazon GameLift works very well for hosting fully custom game servers with plenty of detailed game logic, there are many types of multiplayer games that don't need that level of complexity\. For these games, Realtime Servers offers a simplified path to getting the game up and running for players\. Realtime Servers eliminates the need to develop, test, and deploy a custom game server; instead, by using Realtime servers, which come with core game server infrastructure built in, game developers can avoid a significant amount of work\. At the same time, Realtime servers have the flexibility to incorporate custom game logic\. 

The Realtime Servers option is a good choice for games that don't rely on complex computational gaming and do not require hardware\-intensive game servers\. Games that use Realtime Servers to best effect include lightweight games or games that manage a high percentage of the computational work on the game client\. Examples include messaging games, turn\-based strategy games, and many types of mobile games\. Games that have a very low tolerance for some player latency, such as first person shooters and other fast\-action games, are better served by robust custom game servers, although Amazon GameLift FleetIQ helps minimize latency for all types of game servers\. 

## Key Components<a name="realtime-howitworks-components"></a>

When working with Realtime Servers, you work with the following components\. Learn more about these components and how they work together in [Game Architecture with Realtime Servers](realtime-architecture.md)\.
+ A **Realtime server** provides basic networking functionality for your multiplayer game\. It comes pre\-equipped with the functionality needed to interact with the Amazon GameLift service in order to host game sessions and players\. It allows players to communicate with each other and exchange information in order to maintain a synchronized game state\. It also maintains communication with the Amazon GameLift service to start and stop games, validate players, and report on game health status\. Amazon GameLift deploys Realtime servers to your virtual computing resources for hosting\.
+ A **game client** is your game's software running on a player's device\. The game client triggers requests to the Amazon GameLift service to discover game sessions to join or start new ones, and connects to a Realtime server to participate in a game\. Once connected, a game client can send and receive updates with other players in the game, via the Realtime server, to synchronize game state\.
+ A **Realtime script** provides custom game logic for your game\. The script may involve minimal configuration settings or more complex game logic\. The Realtime script is deployed to hosting resources along with the Realtime server\. Scripts are written in Node\.js\-based JavaScript\. 
+ The **Amazon GameLift service** manages the computing resources needed to host your Realtime servers and makes it possible for players to connect to games\. It regulates the number of resources for player demand, triggers game session starts and stops, and handles player join requests by finding and reserving player slots in active game sessions\. The service also collects performance data on Realtime server health and player usage\. 
+ A **game session** is hosted by an instance of the Realtime server and script, running on hosting resources\. Players connect to a game session to play the game and interact with the other players\. 

## How Realtime Servers Manages Game Sessions<a name="realtime-howitworks-servers"></a>

Amazon GameLift manages game sessions with Realtime Servers in the same way that it handles them with fully custom game servers\. Players, using a game client, send requests to create new game sessions or to find and join existing game sessions\. Most methods for creating game sessions, including game session placement and FlexMatch matchmaking, are available with Realtime Servers \(match backfill is not yet available\)\.

To host game sessions, you set up a fleet of hosting resources to run Realtime servers and configure how the servers will host game sessions\. Amazon GameLift's core features for managing hosting resources, including using autoscaling to manage fleet capacity, are available with Realtime Servers\. 

All the functionality a game server needs to communicate with the Amazon GameLift service is built into a Realtime server\. The Realtime server responds to triggers from Amazon GameLift to start new game sessions, and provides access to accompanying game data or player data \(such as for matchmaking\)\. When a game uses player sessions to reserve game slots or to authenticate player connections, a Realtime server validates players when they connect\. A Realtime server reports its health status back to the Amazon GameLift service, reports when a game session has ended, and responds to prompts from Amazon GameLift to force a game session termination when needed\.

You can also add custom logic to your Realtime servers by building it into the Realtime script\. Access server\-specific objects, including message content and game session objects\. Add event\-driven logic using callbacks, such as to trigger activity when a game session starts or ends\. Add logic based on non\-event scenarios, such as a timer or status check\. 

## How Realtime Clients and Servers Interact<a name="realtime-howitworks-interactions"></a>

During a game session, the interaction between game clients is done by messaging\. Messages are sent and received via the Realtime server\. Game clients use messages to exchange activity, game state, and relevant game data with other game clients in the game\. A Realtime server can also receive messages, such as to trigger game logic in the Realtime script, and initiate messages to game clients\.

Game clients communicate via the Realtime Client SDK, which is integrated into the game client\. The Client SDK defines a set of synchronous API calls that allow clients to connect to games, send messages, and disconnect from games\. It also defines a set of asynchronous callbacks, which may be implemented on the game client, that enables the client to respond to certain events\. 

Realtime servers automatically pass messaging between clients\. In addition, the Realtime script may be customized with additional game logic, such as using callbacks to define event\-driven responses\. 

**Communication Protocol**  
Communication between a Realtime server and connected game clients uses two channels: a TCP connection for reliable delivery and a UDP channel for fast delivery\. When creating messages, game clients choose which protocol to use depending on the nature of the message\. Message delivery is set to UDP by default\. If a UDP channel is not set up or not available, all messages are sent using TCP as a fallback\.

**Message Content**  
Message content consists of two elements: a required operation code \(opCode\) and an optional payload\. A message's opCode identifies a particular player activity or game event, while the payload provides additional data, as needed, related to the operation code\. Both of these elements are developer\-defined; that is, you define what actions map to which opCodes, and whether a message payload is needed\. 

**Player Groups**  
Realtime Servers provides functionality to manage groups of players\. By default, all players who are connected to a game are placed in an "all players" group\. In addition, developers can set up additional groups for their games, and players can be members of multiple groups simultaneously\. Group members can send messages to all players in the group or share game data with the group\. One way to use groups is to to manage teams and team communication\.

## Customizing a Realtime Server<a name="realtime-howitworks-scripts"></a>

In its most basic form, a Realtime server performs as a stateless relay server\. The server relays packets of game data and messages between the game clients that are connected to the game\. Used in this way, each game client maintains a view of the game state and provides updates to other players via the relay server\. Each game client is responsible for incorporating these updates and reconciling its own game state\. By itself, the Realtime server does not process any data or perform any gameplay logic\. 

Alternatively, you can customize your servers by building out the Realtime script functionality\. The simplest relay server, which requires only a minimal Realtime script, allows all messages and all requests to pass through unchallenged\. However, there are many server\-side processes you may choose to implement even while taking advantage of the simplicity of the Realtime Servers feature\. With game logic, for example, you might opt to build a stateful game with a server\-authoritative view of the game state\.

A set of server\-side callbacks are defined for Realtime scripts\. Implement these callbacks to add event\-driven functionality to your server\. For example, you might:
+ Authenticate a player when a game client tries to connect to the server\.
+ Validate whether a player can join a group when requested\.
+ Evaluate when to deliver messages from a certain player or to a target player, or perform additional processing in response\.
+ Take additional action when players leave a group or disconnect from the server\.
+ Evaluate the content of game session objects or message objects and use the data\.

## Deploying and Updating Realtime Servers<a name="realtime-howitworks-deployment"></a>

Realtime Servers is powered by Amazon GameLift’s dedicated server resources with the same high level of stability and security\. As with all servers, latency can be minimized by using Amazon GameLift’s matchmaking and queues with Fleet IQ, which optimizes game session placement based on player locations\.

When deploying Realtime Servers games with Amazon GameLift, the process is nearly identical to deploying traditional game servers on Amazon GameLift\. You create fleets of computing resources and deploy them with a Realtime script instead of a custom game build\. You have control over how server processes are started and run on your fleets, and you can manage fleet capacity as needed\. The detailed description of game hosting in [How Amazon GameLift Works](gamelift-howitworks.md) accurately represents how hosting with Realtime Servers works\. 

An additional advantage with Realtime Servers primary difference is that, unlike custom game server builds, Realtime scripts can be updated at any time without having to deploy new fleets\. When you update a script, the new script is propagated to all hosting resources within a few minutes\. Once the new script is deployed, all new game sessions created after that point will use the new script version \(existing game sessions will continue to use their original version\)\. 