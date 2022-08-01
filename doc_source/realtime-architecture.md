# Game architecture with Realtime Servers<a name="realtime-architecture"></a>

The diagram shown below illustrates the key components of a game architecture that's hosted using the managed GameLift with Realtime Servers solution\.

![\[Game architecture with Realtime Servers\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/realtime_architecture.png)

Key components:

**Game clients**  
To join a game hosted on GameLift, your game client must first find an available game session\. The game client searches for existing game sessions, requests matchmaking, or starts a new game session by communicating with GameLift service through a backend service\. The backend service makes requests to the GameLift service and in response receives game session information, including connection details, which it relays back to the game client\. The game client then uses this information, with the Realtime Client SDK, to connect directly to the game server\. The game client can then join the game and exchange game state updates with other players in the game\. The green arrow represents the direct connection between game client and game server during gameplay\.

**Backend services**  
A backend service handles communication between game clients and the GameLift service by calling the GameLift service APIs in the AWS SDK\.You can also use backend services for other game\-specific tasks such as player authentication and authorization, inventory, or currency control\. 

**External services**  
Your game may rely on an external service, such as for validating a subscription membership\. As shown in the architecture diagram, An external service can pass information to your game servers through a backend service and the GameLift service without going through the game client\.

**Realtime Servers**  
To host game sessions, you create a Realtime Servers fleet that's configured for your game\. The Realtime servers run a script, which you customize for your game and upload to GameLift\. Realtime servers track player connections to a game session and relay game data between players to keep each player's game state in sync\. They also communicate with the GameLift service to start new game sessions, validate newly connected players, and report on the status of game sessions, player connections, and available resources\. When joining a game, a game client connects directly to a Realtime server after receiving connection details from the GameLift service through a backend service\.

**GameLift service**  
The GameLift service is the core service\. When setting up and managing hosting resources, game owners use hosting management tools to manage game server builds or scripts, fleets, matchmaking, and queues\. 

**Hosting management tools**  
The GameLift tool set in the AWS SDK or console provides multiple ways for you to configure your game hosting resources, scale capacity to player demand, and monitor the current status of resources\. You can remotely access any individual game server for troubleshooting\. 