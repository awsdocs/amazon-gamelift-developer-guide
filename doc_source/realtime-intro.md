# Integrating games with GameLift Realtime Servers<a name="realtime-intro"></a>

This topic provides an overview of the managed Amazon GameLift with Realtime Servers solution\. The overview explains when this solution is a good fit for your game, and how Realtime Servers supports multiplayer gaming\.

For a complete roadmap to getting your game up and running, see [GameLift managed hosting roadmap](gamelift-quickstart-intro.md)\.

**Tip**  
For more information about trying out GameLift features, see [Getting started with Amazon GameLift](getting-started-intro.md)\.

## What are Realtime servers?<a name="realtime-howitworks-whatis"></a>

Realtime servers are lightweight, ready\-to\-go game servers that GameLift provides for you to use with your multiplayer games\. Realtime servers remove the development, testing, and deployment process of a custom game server\. This solution can help minimize the time and effort required to complete your game\.

**Key features**
+ Full network stack for game client and server interaction
+ Core game server functionality
+ Customizable server logic
+ Live updates to Realtime configurations and server logic
+ FlexMatch matchmaking
+ Flexible control of hosting resources

Set up Realtime servers by creating a fleet and providing a configuration script\. For more information about creating Realtime servers and how to prepare your game client, see [Prepare your Realtime server](gamelift_quickstart_integration.md#realtime-plan)\.

## How Realtime Servers manages game sessions<a name="realtime-howitworks-servers"></a>

You can add custom logic for game session management by building it into the Realtime script\. You can write code to access server\-specific objects, add event\-driven logic using callbacks, or add logic based on non\-event scenarios\.

## How Realtime clients and servers interact<a name="realtime-howitworks-interactions"></a>

During a game session, game clients interact by sending messages to the Realtime server through a backend service\. The backend service then relays the messages among game clients to exchange activity, game state, and relevant game data\.

In addition, you can customize how clients and servers interact by adding game logic to the Realtime script\. With custom game logic, a Realtime server might implement callbacks to start event\-driven responses\.

**Communication protocol**  
Realtime servers and connected game clients communicate through two channels: a TCP connection for reliable delivery, and a UDP channel for fast delivery\. When creating messages, game clients choose which protocol to use depending on the nature of the message\. Message delivery is set to UDP by default\. If a UDP channel isn't available, GameLift sends messages using TCP as a fallback\.

**Message content**  
Message content consists of two elements: a required operation code \(opCode\) and an optional payload\. A message's opCode identifies a particular player activity or game event, and the payload provides additional data related to the operation code\. Both of these elements are developer\-defined\. Your game client acts based on the opCodes in the messages that it receives\.

**Player groups**  
Realtime Servers provides functionality to manage groups of players\. By default, GameLift places all players who connect to a game in an "all players" group\. In addition, developers can set up other groups for their games, and players can be members of multiple groups simultaneously\. Group members can send messages and share game data with all players in the group\. One possible use for groups is to set up player teams and manage team communication\.

**Realtime Servers with TLS certificates**  
With Realtime Servers, server authentication and data packet encryption are built into the service\. These security features are enabled when you turn on TLS certificate generation\. When a game client tries to connect with a Realtime server, the server automatically responds with the TLS certificate, which the client validates\. GameLift handles encryption using TLS for TCP \(WebSockets\) communication and DTLS for UDP traffic\.

## Customizing a Realtime server<a name="realtime-howitworks-scripts"></a>

A Realtime server performs as a stateless relay server\. The Realtime server relays packets of messages and game data between the game clients connected to the game\. However, the Realtime server doesn't evaluate messages, process data, or perform any gameplay logic\. Used in this way, each game client maintains its own view of the game state and provides updates to other players through the relay server\. Each game client is responsible for incorporating these updates and reconciling its own game state\.

You can customize your servers by adding to the Realtime script functionality\. With game logic, for example, you can build a stateful game with a server\-authoritative view of the game state\.

GameLift defines a set of server\-side callbacks for Realtime scripts\. Implement these callbacks to add event\-driven functionality to your server\. For example, you can:
+ Authenticate a player when a game client tries to connect to the server\.
+ Validate whether a player can join a group upon request\.
+ Determine when to deliver messages from a certain player or to a target player, or perform additional processing in response\.
+ Notify all players when a player leaves a group or disconnects from the server\.
+ View the content of game session objects or message objects, and use the data\.

## Deploying and updating Realtime Servers<a name="realtime-howitworks-deployment"></a>

A key advantage of Realtime Servers is the ability to update your scripts at any time\. When you update a script, GameLift distributes the new version to all hosting resources within minutes\. After GameLift deploys the new script, all new game sessions created after that point will use the new script version\. \(Existing game sessions will continue to use the original version\.\) 

**Topics**
+ [What are Realtime servers?](#realtime-howitworks-whatis)
+ [How Realtime Servers manages game sessions](#realtime-howitworks-servers)
+ [How Realtime clients and servers interact](#realtime-howitworks-interactions)
+ [Customizing a Realtime server](#realtime-howitworks-scripts)
+ [Deploying and updating Realtime Servers](#realtime-howitworks-deployment)
+ [Integrating a game client for Realtime Servers](realtime-client.md)
+ [Creating a Realtime script](realtime-script.md)