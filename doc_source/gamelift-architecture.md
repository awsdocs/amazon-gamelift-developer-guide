# Game architecture with managed GameLift<a name="gamelift-architecture"></a>

The diagram shown below illustrates the key components of a game architecture that's hosted using the managed GameLift solution\.

![\[Game architecture with managed GameLift\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/game_architecture.png)

Key components:

**Game clients**  
To join a game being hosted on GameLift, your game client must first find an available game session\. The game client searches for existing game sessions, requests matchmaking, or starts a new game session by communicating with GameLift service through a backend service\. The backend service makes requests to the GameLift service and in response receives game session information, including connection details, which it relays it back to the game client\. The game client then uses this information to connect directly to the game server and join the game\. The green arrow represents the direct connection between game client and game server during gameplay\.

**Backend services**  
A backend service handles communication between game clients and the GameLift service by calling the GameLift service APIs in the AWS SDK\. You can also use backend services for other game\-specific tasks such as player authentication and authorization, inventory, or currency control\. 

**External services**  
Your game can rely on an external service, such as for validating a subscription membership\. An external service can pass information to your game servers through a backend service and the GameLift service without going through the game client\.

**Game servers**  
You upload your game server software to the GameLift service and GameLift deploys it onto hosting machines to host game sessions and accept player connections\. Game servers communicate with the GameLift service by using the GameLift Server SDK, exchanging requests to start new game sessions, validate newly connected players, and report status of game sessions, player connections, and available resources\. Game clients connect directly to a game server after receiving connection details from the GameLift service through a backend service\. 

**GameLift service**  
The GameLift service is the core service\. When setting up and managing hosting resources, game owners use hosting management tools to manage game server builds or scripts, fleets, matchmaking, and queues\. 

**Hosting management tools**  
The GameLift tool set in the AWS SDK or console provides multiple ways for you to configure your game hosting resources, scale capacity to player demand, and monitor the current status of resources\. You can remotely access any individual game server for troubleshooting\. 