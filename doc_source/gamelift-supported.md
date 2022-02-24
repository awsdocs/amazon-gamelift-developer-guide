# GameLift SDKs<a name="gamelift-supported"></a>

This topic describes the SDKs for use with managed GameLift solutions for custom game server builds and Realtime Servers\. To learn more about other GameLift solutions, see [What Is Amazon GameLift?](gamelift-intro.md)\. 

Use GameLift software development kits \(SDKs\) to develop GameLift\-enabled multiplayer game servers, game clients and game services that need to communicate with the GameLift service\. 

For detailed information on using the GameLift SDKs with your game engine, see [Game Engines and Amazon GameLift](integration-engines.md)\. For the latest version information on GameLift SDKs and SDK compatibility, see [GameLift release notes](release-notes.md)\.

## For custom game servers<a name="gamelift-supported-servers"></a>

Create and deploy 64\-bit custom game servers with the *GameLift Server SDK*\. This SDK enables the GameLift service to deploy and manage game server processes across your GameLift hosting resources\. [Download the Server SDK](https://aws.amazon.com/gamelift/getting-started/) and learn about how to [Add GameLift to your game server](gamelift-sdk-server-api.md) projects\. See [GameLift release notes](release-notes.md) for version\-specific information\.

**SDK support**

The GameLift Server SDK download contains source for the following versions\. Build the version you need for your game; see the README files with each version for build instructions and minimum requirements\.
+ C\+\+
+ C\+\+ for Unreal Engine \(plugin\)
+ C\# \(\.NET\) 

**Development environments**

Build the SDK from source as needed for these supported development operating systems and game engines\.
+ **Operating systems** – Windows, Linux
+ **Game engines** – Amazon Lumberyard, Unreal Engine, Unity, engines that support C\+\+ or C\# libraries

**Game server operating systems**

Use the GameLift Server SDK to create game servers that run on the following platforms: 
+ [Windows Server 2012 R2](https://aws.amazon.com/windows/products/ec2/server2012r2/)
+ [Amazon Linux](https://aws.amazon.com/amazon-linux-ami/)
+ [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/)

## For Realtime Servers<a name="gamelift-supported-realtime"></a>

Configure and deploy Realtime servers to host your multiplayer games, and enable your game clients to connect to them with the *GameLift Realtime Client SDK*\. Game clients use this SDK to exchange messages with a Realtime server and with other game clients that are connected to the server\. [Download the Realtime Client SDK](https://aws.amazon.com/gamelift/getting-started/) and learn about [how to use it with your game clients](realtime-client.md)\. 

**SDK support**

The Realtime Client SDK contains source for the following languages: 
+ C\# \(\.NET\) 

**Development environments**

Build the SDK from source as needed for these supported development operating systems and game engines\.
+ **Operating systems** – Windows, Linux, Android, iOS\.
+ **Game engines** – Unity, engines that support C\# libraries

**Game server operating systems**

Realtime servers are deployed onto hosting resources that run the following platforms: 
+ [Amazon Linux](https://aws.amazon.com/amazon-linux-ami/)
+ [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/)

## For client services<a name="gamelift-supported-clients"></a>

Create 64\-bit client services using the AWS SDK with the GameLift API\. This SDK enables client services to find or create game sessions and join players to games that are being hosted on GameLift\. [Download the AWS SDK](https://aws.amazon.com/tools/#sdk) or [view the GameLift API reference documentation](https://docs.aws.amazon.com/gamelift/latest/apireference/)\.

**SDK support**

The AWS SDK with Amazon GameLift is available in the following languages\. See documentation for each language for details on support for development environments\.
+ C\+\+ \([SDK docs](https://aws.amazon.com/sdk-for-cpp/)\) \([Amazon GameLift](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift.html)\)
+ Java \([SDK docs](https://aws.amazon.com/sdk-for-java/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-java/latest/reference/index.html?com/amazonaws/services/gamelift/AmazonGameLift.html)\)
+ \.NET \([SDK docs](https://aws.amazon.com/sdk-for-net/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/GameLift/NGameLift.html)\)
+ Go \([SDK docs](https://aws.amazon.com/sdk-for-go/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-go/api/service/gamelift/)\)
+ Python \([SDK docs](https://aws.amazon.com/sdk-for-python/)\) \([Amazon GameLift](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/gamelift.html)\)
+ Ruby \([SDK docs](https://aws.amazon.com/sdk-for-ruby/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/GameLift.html)\)
+ PHP \([SDK docs](https://aws.amazon.com/sdk-for-php/)\) \([Amazon GameLift](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.GameLift.GameLiftClient.html)\)
+ JavaScript/Node\.js \(\([SDK docs](https://aws.amazon.com/sdk-for-node-js/)\) \([Amazon GameLift](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/GameLift.html)\)