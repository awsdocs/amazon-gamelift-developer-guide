# Integrating your game server for Amazon GameLift<a name="gamelift-sdk-server"></a>

Your custom game server needs to interact with the GameLift service, and potentially other resources, once it is deployed and running on GameLift instances\. This section provides guidance on how to integrate your game server software with GameLift\.

Integrating your game server is Step 2 on the [Get started with custom servers](gamelift-integration.md) roadmap\. These topics assume that you've created an AWS account and have an existing game server project\.

The topics in this section describe how to handle the following integration tasks: 
+ Establish communication between the GameLift service and your deployed and running game servers\.
+ Get a TLS certificate to establish a secure connection between game client and game server\.
+ Enable your game server software, when deployed on a GameLift instance, to interact with other AWS resources\.
+ Allow game server processes to get information about the fleet they are running on\.

Topics
+ [Add GameLift to your game server](gamelift-sdk-server-api.md)
+ [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)
+ [Get fleet data for a GameLift instance](gamelift-sdk-server-fleetinfo.md)
+ GameLift Server SDK references:
  + [GameLift server API reference for C\+\+](integration-server-sdk-cpp-ref.md)
  + [GameLift server API reference for C\#](integration-server-sdk-csharp-ref.md)
  + [GameLift server API reference for Unreal Engine](integration-server-sdk-unreal-ref.md)