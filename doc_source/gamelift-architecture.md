# Game architecture with managed GameLift<a name="gamelift-architecture"></a>

The following diagram illustrates the key components of a game architecture that's hosted using the managed GameLift solution\.

![\[Game architecture with managed GameLift.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/game_architecture.png)

The key components of this architecture include the following:

**Game clients**  
To join a game hosted on GameLift, your game client must first find an available game session\. The game client searches for existing game sessions, requests matchmaking, or starts a new game session by communicating with GameLift through a backend service\. The backend service makes requests to GameLift, and in response, the service receives game session information, which it relays back to the game client\. The game client then connects to the game server\. For more information, see [Preparing games for Amazon GameLift](integration-intro.md)\.

**Backend services**  
A backend service handles communication between game clients and GameLift by calling the GameLift service API operations in the AWS SDK\. You can also use backend services for other game\-specific tasks such as player authentication and authorization, inventory, or currency control\. For more information, see [Design your backend service](gamelift_quickstart_customservers_designbackend.md)\.

**External services**  
Your game can rely on an external service, such as for validating a subscription membership\. An external service can pass information to your game servers through a backend service and GameLift\.

**Game servers**  
You upload your game server software to GameLift, and GameLift deploys it onto hosting machines to host game sessions and accept player connections\. Game servers communicate with GameLift to start game sessions, validate newly connected players, and report the status of game sessions, player connections, and available resources\.  
Custom game servers communicate with GameLift by using the GameLift Server SDK\. Game clients connect directly to a game server after receiving connection details from GameLift through a backend service\. For more information, see [Integrate games with custom game servers](integration-custom-intro.md)\.  
Realtime servers are game servers that run your custom script\. When joining a game, a game client connects directly to a Realtime server using the Realtime Client SDK\. For more information, see [Integrating games with GameLift Realtime Servers](realtime-intro.md)\.

**Host management tools**  
When setting up and managing hosting resources, game owners use hosting management tools to manage game server builds or scripts, fleets, matchmaking, and queues\. The GameLift tool set in the AWS SDK and the console provides multiple ways for you to manage your hosting resources\. You can remotely access any individual game server for troubleshooting\.