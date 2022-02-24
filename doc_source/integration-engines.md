# Game Engines and Amazon GameLift<a name="integration-engines"></a>

You can use the managed GameLift service with most major game engines that support C\+\+ or C\# libraries, including Amazon Lumberyard, Unreal Engine, and Unity\. Build the version you need for your game; see the README files with each version for build instructions and minimum requirements\. For more information on available GameLift SDKs, supported development platforms and operating systems, see [GameLift SDKs](gamelift-supported.md) for game servers\.

In addition to the engine\-specific information provided in this topic, find additional help with integrating GameLift into your game servers, clients and services in the following topics:
+ [Get Started with Custom Servers](gamelift-integration.md) – A six\-step workflow for successfully integrating GameLift into your game and setting up hosting resources\. 
+ [Add GameLift to your game server](gamelift-sdk-server-api.md) – Detailed instructions on integrating GameLift into a game server\.
+ [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md) – Detailed instructions on integrating into a game client or service, including creating game sessions and joining players to games\.

## Amazon Lumberyard<a name="integration-engines-lumberyard"></a>

GameLift SDKs and functionality are fully incorporated into the Lumberyard product\. 

**Game servers**  
Prepare your game servers for hosting on GameLift using the [GameLift Server SDK for C\+\+](integration-server-sdk-cpp-ref.md)\. See [Add GameLift to your game server](gamelift-sdk-server-api.md) to get help with integrating the required functionality into your game server\.

**Game clients and services**  
Enable your game clients and/or game services to interact with GameLift service, such as to find available game sessions or create new ones, and add players to games\. Core client functionality is provided in the [AWS SDK for C\+\+](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift.html)\. To integrate GameLift into your Lumberyard game project, see [Add GameLift to an Amazon Lumberyard game client](game-client-intro.md) and [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)\.

## Unreal Engine<a name="integration-engines-unreal"></a>

**Game servers**  
Prepare your game servers for hosting on GameLift by adding the [GameLift Server SDK for Unreal Engine](integration-server-sdk-unreal-ref.md) to your project and implementing the required server functionality\. For help setting up the Unreal Engine plugin and adding GameLift code, see [Add Amazon GameLift to an Unreal Engine Game Server Project](integration-engines-setup-unreal.md)\.

**Game clients and services**  
Enable your game clients and/or game services to interact with GameLift service, such as to find available game sessions or create new ones, and add players to games\. Core client functionality is provided in the [AWS SDK for C\+\+](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift.html)\. To integrate GameLift into your Unreal Engine game project, see [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)\.

## Unity<a name="integration-engines-unity"></a>

**Game servers**  
Prepare your game servers for hosting on GameLift by adding the [GameLift Server SDK for C\#](integration-server-sdk-csharp-ref.md) to your project and implementing the required server functionality\. For help setting up with Unity and adding GameLift code, see [Add Amazon GameLift to a Unity Game Server Project](integration-engines-unity-using.md)\.

**Game clients and services**  
Enable your game clients and/or game services to interact with GameLift service, such as to find available game sessions or create new ones, and add players to games\. Core client functionality is provided in the [AWS SDK for \.NET](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/)\. To integrate GameLift into your Unity game project, see [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md)\.

## Other Engines<a name="integration-engines-other"></a>

For a full list of the GameLift SDKs available for game servers and clients, see [GameLift SDKs](gamelift-supported.md)\.