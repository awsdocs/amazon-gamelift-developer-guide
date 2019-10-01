# How Realtime Servers Work<a name="realtime-howitworks"></a>

This topic provides an overview of the Amazon GameLift Realtime Servers feature, discusses when it is a good fit for your game, and explains how Realtime Servers supports multiplayer gaming\.

## What are Realtime Servers?<a name="realtime-howitworks-whatis"></a>

Realtime Servers are lightweight, ready\-to\-go game servers that are provided by GameLift for you to use with your multiplayer games\. While many games need a custom game server to handle complex physics and computations, this is overkill for many other games\. Since Realtime Servers eliminate the need to develop, test, and deploy a custom game server, choosing this solution can help minimize the time and effort required to complete your game\. 

Key features include: 
+ **Full network stack for game client/server interaction\.** Realtime Servers make use of TCP and UDP channels for messaging\. With this feature, you can also opt to use built\-in server authentication and data packet encryption by enabling GameLift\-generated TLS certificates\.
+ **Core game server functionality\.** A Realtime server can start \(and stop\) game sessions, manage game and match data, and accept client connections\. A game server maintains a synchronized game session state by receiving game state information from each client and relaying it to other clients in the game session\.
+ **Integrated with the GameLift service\.** The GameLift service triggers the Realtime server to start a game session, validates players when they connect, and collects player connection status and game health state from the game server\. This functionality, which must be integrated into a custom game server, is fully implemented in Realtime Servers\. 
+ **Customizable server logic\.** You can configure your Realtime servers and customize them with server\-side game logic as best fits your game\. Alternatively, provide a minimal configuration and to use them as simple relay servers\. Learn more about [Customizing a Realtime Server](#realtime-howitworks-scripts)\.
+ **Live updates to Realtime configurations and server logic\.** You can update your Realtime server configuration at any time\. GameLift regularly checks for updated configuration scripts, so once you upload a new version, it is quickly deployed to your fleet and used with all new game sessions\.
+ **FlexMatch matchmaking\.** Game clients that use Realtime Servers can make use of all FlexMatch matchmaking features, including for large matches \(backfill is not yet available\)\. 
+ **Flexible control of hosting resources\.** For games that are deployed with Realtime Servers, you can use all GameLift hosting features, including auto\-scaling, multi\-region queues, game session placement with FleetIQ, game session logging, and metrics\. You determine how your hosting resources are utilized \(via the runtime configuration and other controls\)\. 
+ **Range of computing resource options\.** Realtime servers run on Linux\. You choose the type of computing resources \(instance type\) for your fleet and whether to use Spot or On\-Demand instances\. 
+ **AWS reliability\.** As with all of GameLift, hosting resources with Realtime Servers bring the high level of quality, security, and reliability of AWS\.

Set up Realtime servers by creating a fleet of hosting resources and providing a configuration script\. Learn more about creating Realtime servers and how to prepare your game client in [Get Started with Realtime Servers](realtime-plan.md)\.

## Choosing Realtime Servers for Your Game<a name="realtime-howitworks-gametype"></a>

Choosing Realtime Servers instead of building a custom game server primarily comes down to your game's need for server complexity\. Does your game require complicated game logic and split\-second computations? If not, then Realtime Servers may be the better solution for your game\. Games that use Realtime Servers to best effect include lightweight games or games that manage a higher percentage of the computational work on the game client\. Examples include messaging games, turn\-based strategy games, and many types of mobile games\. Realtime Servers, coupled with the use of FleetIQ, provides excellent tools for minimizing player latency, although games that have a very low tolerance for player latency, such as first person shooters and other fast\-action games, may be better served by robust custom game servers\. 

## Key Components<a name="realtime-howitworks-components"></a>

When working with Realtime Servers, you work with the following components\. Learn more about these components and how they work together in [Game Architecture with Realtime Servers](realtime-architecture.md)\.
+ A **Realtime server** provides client/server networking for your game\. It starts game sessions when triggered by the GameLift service, requests validation for players when they connect, and reports back on the status player connections and game health\. The server relays game state data between all connected players, and executes custom game logic if provided\. 
+ A **game client** is your game's software running on a player's device\. The game client \(through a client service\) makes requests to the GameLift service to find game sessions to join or to start new ones, and connects to a Realtime server to participate in a game\. Once connected, a game client can send and receive data, through the Realtime server, with other players in the game\.
+ A **Realtime script** provides configuration settings and optional custom game logic for your game\. The script may contain minimal configuration settings or have more complex game logic\. The Realtime script is deployed along with the Realtime server when starting up new hosting resources\. Scripts are written in Node\.js\-based JavaScript\. 
+ The **GameLift service** manages the computing resources needed to host your Realtime servers and makes it possible for players to connect to games\. It regulates the number of resources for player demand, handles player join requests by finding and reserving player slots in active game sessions, triggers Realtime servers to start game sessions, and validates players when they connect to a game server\. The service also collects metrics on Realtime server health and player usage\. 
+ A **game session** is an instance of your game, run on a Realtime server\. Players connect to a game session to play the game and interact with the other players\. 

## How Realtime Servers Manages Game Sessions<a name="realtime-howitworks-servers"></a>

GameLift manages game sessions with Realtime Servers in the same way that it handles them with fully custom game servers\. Players, using a game client, send requests to create new game sessions or to find and join existing game sessions\. Most methods for creating game sessions, including game session placement and FlexMatch matchmaking, are available with Realtime Servers \(match backfill is not yet available\)\.

A Realtime server, once it is deployed on a fleet of hosting instances, maintains communication with the GameLift service\. The Realtime server starts a game session when it is prompted to by the GameLift service and receives available game session and player data, including matchmaking data from the service\. If your game uses player sessions to reserve game slots or to authenticate player connections, the Realtime server can send a validation request to the GameLift service when the player connects\. A Realtime server also reports its health status back to the GameLift service, and notifies the service when players connect/disconnect and when a game session ends\. It also responds to prompts from GameLift to force a game session termination\. This interaction with the GameLift service is fully built into all Realtime Servers\.

You have the option of adding custom logic for game session management by building it into the Realtime script\. You might write code to access server\-specific objects, add event\-driven logic using callbacks, or add logic based on non\-event scenarios, such as a timer or status check\. For example, you might want to or access game session objects, or trigger an action when a game session starts or ends\.

## How Realtime Clients and Servers Interact<a name="realtime-howitworks-interactions"></a>

During a game session, the interaction between game clients in the game is done by messaging\. Game clients use messages to exchange activity, game state, and relevant game data\. Game clients send messages to the Realtime server, which then relays the messages among the game clients\. Game clients communicate with the server using the Realtime Client SDK, which must be integrated into your game client\. The Client SDK defines a set of synchronous API calls that allow clients to connect to games, send and receive messages, and disconnect from games\. It also defines a set of asynchronous callbacks, which can be implemented on the game client to enable the client to respond to certain events\. 

In addition, you can customize how clients and servers interact by adding game logic to the Realtime script\. With custom game logic, a Realtime might implement callbacks to trigger event\-driven responses\. For example, when a game client notifies the server that a certain achievement is reached, the server sends a message to other game clients to prompt an announcement\.

**Communication Protocol**  
Communication between a Realtime server and connected game clients uses two channels: a TCP connection for reliable delivery and a UDP channel for fast delivery\. When creating messages, game clients choose which protocol to use depending on the nature of the message\. Message delivery is set to UDP by default\. If a UDP channel is not set up or not available, all messages are sent using TCP as a fallback\.

**Message Content**  
Message content consists of two elements: a required operation code \(opCode\) and an optional payload\. A message's opCode identifies a particular player activity or game event, while the payload provides additional data, as needed, related to the operation code\. Both of these elements are developer\-defined; that is, you define what actions map to which opCodes, and whether a message payload is needed\. You game client takes action based on the opCodes in the messages it receives\. 

**Player Groups**  
Realtime Servers provides functionality to manage groups of players\. By default, all players who are connected to a game are placed in an "all players" group\. In addition, developers can set up other groups for their games, and players can be members of multiple groups simultaneously\. Group members can send messages to all players in the group or share game data with the group\. One possible use for groups is to set up player teams and manage team communication\.

**Realtime Servers with TLS Certificates**  
You can opt to create Realtime Servers fleets with TLS certificate generation turned on\. GameLift generates a TLS certificate for the fleet and creates a DNS entry for each instance in the fleet\. This allows your game to authenticate the client/server connection and encrypt all game client/server communication\. This feature makes it possible to publish games on a range of platforms, including mobile, that require enhanced security and encrypted communication\. It helps to protect your game clients \(and players\) from server spoofing attacks, and prevents malicious actors from hacking or monitoring data transmissions\. These services are provided through the AWS Certificate Manager \(ACM\) and are currently available at no additional cost\.

With Realtime Servers, server authentication and data packet encryption is already built into the service, and is enabled when you turn on TLS certificate generation\. When the game client tries to connect with a Realtime server, the server automatically responds with the TLS certificate, which the client validates\. Encryption is handled using TLS for TCP \(Websockets\) communication and DTLS for UDP traffic\.

## Customizing a Realtime Server<a name="realtime-howitworks-scripts"></a>

In its most basic form, a Realtime server performs as a stateless relay server\. The Realtime server relays packets of messages and game data between the game clients that are connected to the game, but does not evaluate messages, process data, or perform any gameplay logic\. Used in this way, each game client maintains its own view of the game state and provides updates to other players via the relay server\. Each game client is responsible for incorporating these updates and reconciling its own game state\. 

Alternatively, you can customize your servers by building out the Realtime script functionality\. There are many server\-side processes you may choose to implement even while taking advantage of the simplicity of the Realtime Servers feature\. With game logic, for example, you might opt to build a stateful game with a server\-authoritative view of the game state\.

A set of server\-side callbacks are defined for Realtime scripts\. Implement these callbacks to add event\-driven functionality to your server\. For example, you might:
+ Authenticate a player when a game client tries to connect to the server\.
+ Validate whether a player can join a group when requested\.
+ Evaluate when to deliver messages from a certain player or to a target player, or perform additional processing in response\.
+ Take action, such as notifying all players, when a player leaves a group or disconnects from the server\.
+ Evaluate the content of game session objects or message objects and use the data\.

## Deploying and Updating Realtime Servers<a name="realtime-howitworks-deployment"></a>

Realtime Servers is powered by GameLift’s dedicated server resources\. There is no difference in stability and security provided\. As with all servers, latency can be minimized by using GameLift’s matchmaking and queues with Fleet IQ, which optimizes game session placement based on player locations\.

When deploying Realtime Servers games with GameLift, the process is nearly identical to deploying traditional game servers on GameLift\. You create fleets of computing resources and deploy them with your Realtime script, which contains configuration details and optional custom logic\. Using GameLift, you choose the type of fleets to use, manage fleet capacity, and control how game server processes are started and run on your fleets\. The detailed description of game hosting in [How Amazon GameLift Works](gamelift-howitworks.md) represents game hosting with Realtime Servers as well as with custom game servers\.

A key advantage Realtime Servers is the ability to update your scripts at any time\. You do not need to create a new fleet to deploy an updated script\. When you update a script, the new version is propagated to all hosting resources within a few minutes\. Once the new script is deployed, all new game sessions created after that point will use the new script version \(existing game sessions will continue to use the original version\)\. 