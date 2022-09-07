# Download Amazon GameLift SDKs<a name="gamelift-supported"></a>

This topic describes the SDKs that you can use with managed GameLift solutions for custom game server builds and Realtime Servers\. Use GameLift SDKs to develop multiplayer game servers, game clients, and game services that communicate with GameLift\.

For detailed information about using the GameLift SDKs with your game engine, see [Game engines and Amazon GameLift](integration-engines.md)\. For the latest information about GameLift SDK versions and SDK compatibility, see [GameLift release notes](release-notes.md)\.

## For custom game servers<a name="gamelift-supported-servers"></a>

Create and deploy 64\-bit custom game servers with the GameLift Server SDK\. This SDK enables GameLift to deploy and manage game server processes across your GameLift hosting resources\. To get started, [download the GameLift Managed Servers SDK](https://aws.amazon.com/gamelift/getting-started/)\. For configuration information, see [Add GameLift to your game server](gamelift-sdk-server-api.md)\.

**SDK support**

The GameLift Managed Servers SDK download contains source for the following versions\. Build the version that you need for your game\. For build instructions and minimum requirements, see the README files for each version\.
+ C\+\+
+ C\+\+ for Unreal Engine \(plugin\)
+ C\# \(\.NET\)

**Development environments**

Build the SDK from source as needed for the following supported development operating systems and game engines:
+ **Operating systems** – Windows, Linux
+ **Game engines** – Amazon Lumberyard, Unreal Engine, Unity, engines that support C\+\+ or C\# libraries

**Game server operating systems**

Use the GameLift Server SDK to create game servers that run on the following platforms:
+ [Windows Server 2012 R2](http://aws.amazon.com/windows/products/ec2/server2012r2/)
+ [Amazon Linux](http://aws.amazon.com/amazon-linux-ami/)
+ [Amazon Linux 2](http://aws.amazon.com/amazon-linux-2/)

## For Realtime Servers<a name="gamelift-supported-realtime"></a>

Configure and deploy Realtime servers to host your multiplayer games\. To enable your game clients to connect to Realtime servers, use the GameLift Realtime Client SDK\. Game clients use this SDK to exchange messages with a Realtime server and with other game clients that connect to the server\. To get started, [download the GameLift Realtime Client SDK](http://aws.amazon.com/gamelift/getting-started/)\. For configuration information, see [Integrating a game client for Realtime Servers](realtime-client.md)\.

**SDK support**

The Realtime Client SDK contains source for the following languages:
+ C\# \(\.NET\)

**Development environments**

Build the SDK from source as needed for the following supported development operating systems and game engines:
+ **Operating systems** – Windows, Linux, Android, iOS
+ **Game engines** – Unity, engines that support C\# libraries

**Game server operating systems**

You can deploy Realtime servers onto hosting resources that run on the following platforms:
+ [Amazon Linux](http://aws.amazon.com/amazon-linux-ami/)
+ [Amazon Linux 2](http://aws.amazon.com/amazon-linux-2/)

## For client services<a name="gamelift-supported-clients"></a>

Create 64\-bit client services using the AWS SDK with the GameLift API\. This SDK enables client services to find or create game sessions and to join players to games that are hosted on GameLift\. To get started, [download the AWS SDK](http://aws.amazon.com/developer/tools/#SDKs)\. For more information about using the SDK with GameLift, see the [Amazon GameLift API Reference](https://docs.aws.amazon.com/gamelift/latest/apireference/Welcome.html)\.

**SDK support**

The AWS SDK with support for GameLift is available in the following languages\. For information about support for development environments, see the documentation for each language\.
+ C\+\+ \([SDK docs](http://aws.amazon.com/sdk-for-cpp/)\) \([GameLift](https://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift.html)\)
+ Java \([SDK docs](http://aws.amazon.com/sdk-for-java/)\) \([GameLift](https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/services/gamelift/package-summary.html)\)
+ \.NET \([SDK docs](http://aws.amazon.com/sdk-for-net/)\) \([GameLift](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/GameLift/NGameLift.html)\)
+ Go \([SDK docs](http://aws.amazon.com/sdk-for-go/)\) \([GameLift](https://docs.aws.amazon.com/sdk-for-go/api/service/gamelift/)\)
+ Python \([SDK docs](http://aws.amazon.com/sdk-for-python/)\) \([GameLift](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/gamelift.html)\)
+ Ruby \([SDK docs](http://aws.amazon.com/sdk-for-ruby/)\) \([GameLift](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/GameLift.html)\)
+ PHP \([SDK docs](http://aws.amazon.com/sdk-for-php/)\) \([GameLift](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.GameLift.GameLiftClient.html)\)
+ JavaScript/Node\.js \([SDK docs](http://aws.amazon.com/sdk-for-node-js/)\) \([GameLift](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-gamelift/index.html)\)