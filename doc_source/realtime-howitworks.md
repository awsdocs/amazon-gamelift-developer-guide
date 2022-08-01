# How Realtime Servers work<a name="realtime-howitworks"></a>

This topic provides an overview of the managed Amazon GameLift with Realtime Servers solution\. It discusses when it's a good fit for your game, and explains how Realtime Servers supports multiplayer gaming\. To learn more about other GameLift solutions, see [What is Amazon GameLift?](gamelift-intro.md)\. 

## What are Realtime Servers?<a name="realtime-howitworks-whatis"></a>

Realtime Servers are lightweight, ready\-to\-go game servers that GameLift provides for you to use with your multiplayer games\. While many games need a custom game server to handle complex physics and computations, this isn't needed for other games\. Because Realtime Servers eliminate the development, testing, and deployment of a custom game server, choosing this solution can help minimize the time and effort required to complete your game\. 

Key features include: 
+ **Full network stack for game client and server interaction\.** Realtime Servers use TCP and UDP channels for messaging\. You can also use built\-in server authentication and data packet encryption by enabling TLS certificates\.
+ **Core game server functionality\.** A Realtime server starts and stops game sessions, manages game and match data, and accepts client connections\. The game server maintains a synchronized game session state by receiving game state information from each client and relaying it to other clients in the game session\.
+ **Integrated with the GameLift service\.** A Realtime server communicates with the GameLift service, which tells the Realtime server to start game sessions, validate players when they connect, and collects player connection status and game health state from the game server\.
+ **Customizable server logic\.** You can configure your Realtime servers and customize them with server\-side game logic as best fits your game\. Or, provide a minimal configuration and to use them as simple relay servers\. Learn more about [Customizing a Realtime server](#realtime-howitworks-scripts)\.
+ **Live updates to Realtime configurations and server logic\.** Update your Realtime server configuration at any time\. GameLift regularly checks for updated configuration scripts and deploys them to your fleet\. 
+ **Range of computing resource options\.** Realtime servers run on Linux\. You choose the type of computing hardware for your fleet and whether to use Spot or On\-Demand instances\. 
+ **FlexMatch matchmaking\.** Game clients that use Realtime Servers can make use of all FlexMatch matchmaking features, including for large matches\. 
+ **Flexible control of hosting resources\.** For games that are deployed with Realtime Servers, you can use all GameLift management features, including auto\-scaling, multi\-region queues, game session placement with FleetIQ, game session logging, and metrics\. You determine how your hosting resources are utilized\. 

Set up Realtime servers by creating a fleet and providing a configuration script\. Learn more about creating Realtime servers and how to prepare your game client in [Get started with Realtime Servers](realtime-plan.md)\.

## Choosing Realtime Servers for your game<a name="realtime-howitworks-gametype"></a>

Choosing Realtime Servers instead of building a custom game server primarily is determined by your game's need for server complexity\. Realtime Servers are best for lighter weight games or games that manage a higher percentage of the computational work on the game client\. Examples include messaging games, turn\-based strategy games, and many types of mobile games\.

## Key components<a name="realtime-howitworks-components"></a>

When working with Realtime Servers, you work with the following components\. Learn more about these components and how they work together in [Game architecture with Realtime Servers](realtime-architecture.md)\.
+ A **Realtime server** provides client and server networking for your game\. It starts game sessions when notified by the GameLift service, requests validation for players when they connect, and reports back on the status player connections and game health\. The server relays game state data between all connected players, and executes custom game logic if provided\. 
+ A **game client** is your game's software running on a player's device\. The game client through a backend service makes requests to the GameLift service to find game sessions to join or to start new ones, and connects to a Realtime server to participate in a game\. Once connected, a game client can send and receive data, through the Realtime server, with other players in the game\.
+ A **Realtime script** provides configuration settings and optional custom game logic for your game\. The script may contain minimal configuration settings or have more complex game logic\. GameLift deploys the Realtime script along with the Realtime server when starting up new hosting resources\. You write scripts in Node\.js\-based JavaScript\. 
+ The **GameLift service** manages the computing resources needed to host your Realtime servers and makes it possible for players to connect to games\. It regulates the number of resources for player demand, handles player join requests by finding and reserving player slots in active game sessions, initiates Realtime servers to start game sessions, and validates players when they connect to a game server\. The service also collects metrics on Realtime server health and player usage\. 
+ **Backend services** are additional custom services that handle tasks related to GameLift\. As a best practice, a backend service handles all game client communication with the GameLift service\. Use backend services for enhanced security and efficiency\.
+ A **game session** is an instance of your game, run on a Realtime server\. Players connect to a game session to play the game and interact with the other players\. 

## How Realtime Servers manages game sessions<a name="realtime-howitworks-servers"></a>

GameLift manages game sessions with Realtime Servers in the same way that it handles game sessions with fully custom game servers\. Players, using a game client, send requests to create new game sessions or to find and join existing game sessions\. Most methods for creating game sessions, including game session placement and FlexMatch matchmaking, are available with Realtime Servers \(match backfill isn't available\)\.

You can add custom logic for game session management by building it into the Realtime script\. You can write code to access server\-specific objects, add event\-driven logic using callbacks, or add logic based on non\-event scenarios, such as a timer or status check\. For example, you can access game session objects to initiate an action when a game session starts or ends\.

## How Realtime clients and servers interact<a name="realtime-howitworks-interactions"></a>

During a game session, game clients in the game interact by messaging\. Game clients use messages to exchange activity, game state, and relevant game data\. Game clients send messages to the Realtime server through a backend services, which then relays the messages among the game clients\. Game clients communicate with the server and backend services using the Realtime client SDK, which you integrate into your game client\. The Client SDK defines a set of synchronous API calls that allow clients to connect to games, send and receive messages, and disconnect from games\. It also defines a set of asynchronous callbacks, which you can implement on the game client so the client can respond to certain events\. 

In addition, you can customize how clients and servers interact by adding game logic to the Realtime script\. With custom game logic, a Realtime server might implement callbacks to start event\-driven responses\. For example, when a game client notifies the server that a player reached an achievement, the server sends a message to other game clients to prompt an announcement\.

**Communication Protocol**  
Communication between a Realtime server and connected game clients uses two channels: a TCP connection for reliable delivery and a UDP channel for fast delivery\. When creating messages, game clients choose which protocol to use depending on the nature of the message\. Message delivery is set to UDP by default\. If a UDP channel isn't available, GameLift sends messages using TCP as a fallback\.

**Message Content**  
Message content consists of two elements: a required operation code \(opCode\) and an optional payload\. A message's opCode identifies a particular player activity or game event, while the payload provides additional data, as needed, related to the operation code\. Both of these elements are developer\-defined\. Your game client takes action based on the opCodes in the messages it receives\. 

**Player Groups**  
Realtime Servers provides functionality to manage groups of players\. By default, GameLift places all players who connected to a game in an "all players" group\. In addition, developers can set up other groups for their games, and players can be members of multiple groups simultaneously\. Group members can send messages to all players in the group or share game data with the group\. One possible use for groups is to set up player teams and manage team communication\.

**Realtime Servers with TLS Certificates**  
You can opt to create Realtime Servers fleets with TLS certificate generation\. GameLift generates a TLS certificate for the fleet and creates a DNS entry for each instance in the fleet\. This allows your game to authenticate the client server connection and encrypt all game client server communication\. This feature makes it possible to publish games on a range of platforms, including mobile, that require enhanced security and encrypted communication\. AWS Certificate Manager \(ACM\) provides these services at no additional cost\.

With Realtime Servers, server authentication and data packet encryption is already built into the service, and enabled when you turn on TLS certificate generation\. When the game client tries to connect with a Realtime server, the server automatically responds with the TLS certificate, which the client validates\. GameLift handles encryption using TLS for TCP \(Websockets\) communication and DTLS for UDP traffic\.

## Customizing a Realtime server<a name="realtime-howitworks-scripts"></a>

A Realtime server performs as a stateless relay server\. The Realtime server relays packets of messages and game data between the game clients connected to the game, but doesn't evaluate messages, process data, or perform any gameplay logic\. Used in this way, each game client maintains its own view of the game state and provides updates to other players through the relay server\. Each game client is responsible for incorporating these updates and reconciling its own game state\. 

You can customize your servers by adding to the Realtime script functionality\. With game logic, for example, you can build a stateful game with a server\-authoritative view of the game state\.

GameLift defines a set of server\-side callbacks for Realtime scripts\. Implement these callbacks to add event\-driven functionality to your server\. For example, you can:
+ Authenticate a player when a game client tries to connect to the server\.
+ Validate whether a player can join a group when requested\.
+ Determine when to deliver messages from a certain player or to a target player, or perform additional processing in response\.
+ Notifying all players, when a player leaves a group or disconnects from the server\.
+ View the content of game session objects or message objects and use the data\.

## Deploying and updating Realtime Servers<a name="realtime-howitworks-deployment"></a>

Realtime Servers is powered by GameLift dedicated server resources\. There is no difference in stability and security provided\. As with all servers, latency can be minimized by using GameLift's matchmaking and queues with Fleet IQ, which optimizes game session placement based on player locations\.

A key advantage Realtime Servers is the ability to update your scripts at any time\. When you update a script, GameLift distributes the new version to all hosting resources within minutes\. After GameLift deploys the new script, all new game sessions created after that point will use the new script version \(existing game sessions will continue to use the original version\)\. 