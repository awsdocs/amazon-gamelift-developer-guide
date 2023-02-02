# Integrate your game server with GameLift<a name="gamelift-sdk-server"></a>

After your custom game server is deployed and running on Amazon GameLift instances, it must be able to interact with GameLift \(and potentially other resources\)\. This section describes how to integrate your game server software with GameLift\.

**Note**  
These instructions assume that you've created an AWS account and that you have an existing game server project\.

The topics in this section describe how to handle the following integration tasks:
+ Establish communication between GameLift and your game servers\.
+ Generate and use a TLS certificate to establish a secure connection between game client and game server\.
+ Provide permissions for your game server software to interact with other AWS resources\.
+ Allow game server processes to get information about the fleet that they're running on\.

**Topics**
+ [Add GameLift to your game server](gamelift-sdk-server-api.md)
+ [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)