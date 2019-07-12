# Game Architecture with Amazon GameLift<a name="gamelift-architecture"></a>

The diagram shown below illustrates the key components of an Amazon GameLift\-hosted game architecture\. 

![\[Game architecture with Amazon GameLift\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/whatis-architecture-vsd.png)

Key components are described as follows\.

**Game clients**  
To join a game being hosted on Amazon GameLift, your game client must first find an available game session\. The game client searches for existing game sessions, requests matchmaking, or starts a new game session by communicating with Amazon GameLift service\. This communication is done through a backend client service in order to help game owners maintain secure control of their game servers and hosting resources\. The client service makes requests to the Amazon GameLift service and in response receives game session information, including connection details, which it relays it back to the game client\. The game client then uses this information to connect directly to the game server and join the game\. The green arrow represents the direct connection between game client and game server during gameplay\.

**Client services**  
A backend client service handles communication between game clients and the Amazon GameLift service by calling the Amazon GameLift service APIs in the AWS SDK\. Client services might also be used for other game\-specific tasks such as player authentication and authorization, inventory, or currency control\. For example, when a player joins a game, your game client might first call an authentication service to first verify the player's identity, and only then send a player slot request to the Amazon GameLift service\. Relevant information, such as connection details, are relayed back to the game client\.

**External services**  
Your game may rely on an external service, such as for validating a subscription membership\. As shown in the architecture diagram, information from an external service can be passed to your game servers \(via a client service and the Amazon GameLift service\) without going through the game client\.

**Game servers**  
Your game server software is uploaded to the Amazon GameLift service and is deployed onto hosting machines to host game sessions and accept player connections\. Game servers communicate with the Amazon GameLift service by using the Amazon GameLift Server SDK, exchanging requests to start new game sessions, validate newly connected players, and report status of game sessions, player connections, and available resources\. Game clients connect directly to a game server after receiving connection details from the Amazon GameLift service\. 

**Amazon GameLift service**  
The Amazon GameLift service is the core service that deploys and manages fleets of resources to host your game servers, coordinates how game sessions are placed across your available resources, starts and stops game sessions, and tracks game server health and activity in order to maintain game availability as player traffic fluctuates\. When setting up and managing hosting resources, game owners use the Amazon GameLift service APIs in the AWS SDK and CLI to upload game server builds, create and configure fleets, and manage fleet capacity\. Client services start new game sessions, request matchmaking, and slot players into game sessions by calling the Amazon GameLift service APIs in the AWS SDK\. Game servers that are deployed onto Amazon GameLift fleets use the Amazon GameLift Server SDK to maintain communication with the Amazon GameLift service to start and stop game sessions, report server health, exchange game and player data as needed, etc\. 

**Hosting management tools**  
The Amazon GameLift tool set in the AWS SDK provides multiple ways for you to configure your game hosting resources, scale capacity to player demand, and monitor the current status of resources, as well as track metrics on game server performance and game and player activity\. In addition, you can remotely access any individual game server for troubleshooting\.