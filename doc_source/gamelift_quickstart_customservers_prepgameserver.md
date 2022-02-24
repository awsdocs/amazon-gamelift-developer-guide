# Prepare Your Custom Game Server<a name="gamelift_quickstart_customservers_prepgameserver"></a>

 The Amazon GameLift Server SDK is available for C\+\+, C\# \(Unity\), and as an Unreal Engine plugin\. You will need to integrate the SDK to your game server build and implement a set of callbacks to communicate with the GameLift service for session management\. 

Download the GameLift Managed Game Servers SDK and build it for your preferred programming language and game engine\. If you are using the Amazon Lumberyard game engine, a version of the SDK is built in\. For more information about GameLift SDKs for custom game servers, see [For custom game servers](gamelift-supported.md#gamelift-supported-servers)\. For detailed information on using the GameLift SDKs with your game engine, see [Game Engines and Amazon GameLift](integration-engines.md)\. For the latest version information on GameLift SDKs and SDK compatibility, see [GameLift release notes](release-notes.md)\. 

Add code to your game server project to enable communication with the GameLift service\. A game server must be able to notify GameLift about its status, to start and stop game sessions when prompted, and to perform other tasks\. For more information, see [Add GameLift to your game server](gamelift-sdk-server-api.md)\. 