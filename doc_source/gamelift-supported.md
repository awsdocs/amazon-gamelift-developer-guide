# Amazon GameLift SDKs<a name="gamelift-supported"></a>

Use Amazon GameLift software development kits \(SDKs\) to develop Amazon GameLift\-enabled multiplayer game servers, game clients and game services that need to communicate with the Amazon GameLift service\. 

For detailed information on using the Amazon GameLift SDKs with your game engine, see [Game Engines and Amazon GameLift](integration-engines.md)\.

## For Custom Game Servers<a name="gamelift-supported-servers"></a>

Create and deploy 64\-bit custom game servers with the *Amazon GameLift Server SDK*\. This SDK enables the Amazon GameLift service to deploy and manage game server processes across your Amazon GameLift hosting resources\. [Download the Server SDK](https://aws.amazon.com/gamelift/getting-started/) and learn about how to [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md) projects\. See the Amazon GameLift [Release Notes](https://aws.amazon.com/releasenotes/amazon-gamelift/) for version\-specific information\.

**SDK support**

The Amazon GameLift Server SDK download contains source for the following versions\. Build the version you need for your game; see the README files with each version for build instructions and minimum requirements\.
+ C\+\+
+ C\+\+ for Unreal Engine \(plugin\)
+ C\# \(\.NET\) 

**Development environments**

Build the SDK from source as needed for these supported development operating systems and game engines\.
+ **Operating systems** – Windows, Linux
+ **Game engines** – Amazon Lumberyard, Unreal Engine, Unity, engines that support C\+\+ or C\# libraries

**Game server operating systems**

Use the Amazon GameLift Server SDK to create game servers that run on the following platforms: 
+ [Windows Server 2012 R2](https://aws.amazon.com/windows/products/ec2/server2012r2/)
+ [Amazon Linux](https://aws.amazon.com/amazon-linux-ami/)

## For Realtime Servers<a name="gamelift-supported-realtime"></a>

Configure and deploy Realtime servers to host your multiplayer games, and enable your game clients to connect to them with the *Amazon GameLift Realtime Client SDK*\. Game clients use this SDK to exchange messages with a Realtime server and with other game clients that are connected to the server\. [Download the Realtime Client SDK](https://aws.amazon.com/gamelift/getting-started/) and learn about [how to use it with your game clients](realtime-client.md)\. 

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

## For Client Services<a name="gamelift-supported-clients"></a>

Create 64\-bit client services using the AWS SDK with the Amazon GameLift API\. This SDK enables client services to find or create game sessions and join players to games that are being hosted on Amazon GameLift\. [Download the AWS SDK](https://aws.amazon.com/tools/#sdk) or [view the Amazon GameLift API reference documentation](https://docs.aws.amazon.com/gamelift/latest/apireference/)\.

**SDK support**

The AWS SDK with Amazon GameLift is available in the following languages\. See documentation for each language for details on support for development environments\.
+ C\+\+ \([SDK docs](https://aws.amazon.com/sdk-for-cpp/)\) \([Amazon GameLift](http://sdk.amazonaws.com/cpp/api/LATEST/namespace_aws_1_1_game_lift.html)\)
+ Java \([SDK docs](https://aws.amazon.com/sdk-for-java/)\) \([Amazon GameLift](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/index.html?com/amazonaws/services/gamelift/AmazonGameLift.html)\)
+ \.NET \([SDK docs](https://aws.amazon.com/sdk-for-net/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/GameLift/NGameLift.html)\)
+ Go \([SDK docs](https://aws.amazon.com/sdk-for-go/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-go/api/service/gamelift/)\)
+ Python \([SDK docs](https://aws.amazon.com/sdk-for-python/)\) \([Amazon GameLift](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/gamelift.html)\)
+ Ruby \([SDK docs](https://aws.amazon.com/sdk-for-ruby/)\) \([Amazon GameLift](https://docs.aws.amazon.com/sdk-for-ruby/v3/api/Aws/GameLift.html)\)
+ PHP \([SDK docs](https://aws.amazon.com/sdk-for-php/)\) \([Amazon GameLift](https://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.GameLift.GameLiftClient.html)\)
+ JavaScript/Node\.js \(\([SDK docs](https://aws.amazon.com/sdk-for-node-js/)\) \([Amazon GameLift](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/GameLift.html)\)

## SDK Compatibility<a name="gamelift-supported-compatibility"></a>

The release history of the Amazon GameLift SDKs is as follows\. There is no requirement to use comparable SDKs for your game server and client integrations, however older versions may not fully support use of the latest features\. 


****  

| Release: | AWS SDK version: | Server SDK version: | Realtime Client SDK version: | 
| --- | --- | --- | --- | 
| [2019\-04\-25](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2019-04-25/) | 1\.3\.58 \([commit](https://github.com/aws/aws-sdk-cpp/commit/b5691e19a630e5b3232312d9e06ac48defa076be)\) or later | 3\.3\.0 | 1\.0\.0 | 
| [2018\-12\-14](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-12-14/) | 1\.3\.58 \([commit](https://github.com/aws/aws-sdk-cpp/commit/b5691e19a630e5b3232312d9e06ac48defa076be)\) or later | 3\.3\.0 | 
| [2018\-02\-15](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-02-15/) | 1\.3\.58 \([commit](https://github.com/aws/aws-sdk-cpp/commit/b5691e19a630e5b3232312d9e06ac48defa076be)\) or later | 3\.2\.1 | 
| [2018\-02\-08](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-02-08/) | 1\.3\.52 \([commit](https://github.com/aws/aws-sdk-cpp/commit/0af9c3f8ffbee156878a1bdfecdc31f84b0af166)\) or later | 3\.2\.0 | 
| [2017\-08\-16](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2017-08-16/) | 1\.1\.31 \([commit](https://github.com/aws/aws-sdk-cpp/commit/8353aa4f7fdebdbc83ef72ed3e0d2542153bf548)\) or later | 3\.1\.7 | 
| [2017\-02\-21](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2017-02-21/) | 1\.0\.72 \([commit](https://github.com/aws/aws-sdk-cpp/commit/b9d5eb702e2ce02632890e00b3d8af55d2bc5202)\) or later | 3\.1\.5  | 
| [2016\-09\-01](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2016-09-01/) |  0\.14\.9 \([commit](https://github.com/aws/aws-sdk-cpp/commit/d460f766d58ee6f8771b932ad4a4e243a9bcfe3a)\) or later  |  3\.1\.0 \(C\+\+ only\)  | 
| [2016\-08\-04](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2016-08-04/) |  0\.12\.16 \([commit](https://github.com/aws/aws-sdk-cpp/commit/6e0d1e23267b7e1b002ea37e520783761d2f1785)\) or later  |  3\.0\.7 \(C\+\+ only\)  | 

\(Version information for the AWS SDK for C\+\+ can be found in this file: `aws-sdk-cpp/aws-cpp-sdk-core/include/aws/core/VersionConfig.h`\. \)

For Amazon Lumberyard users, the following table lists the Amazon GameLift SDK versions that are bundled into or are compatible with the Lumberyard game engine\. 


****  

| Amazon Lumberyard version: | Are bundled with Amazon GameLift SDK versions: | 
| --- | --- | 
| 1\.4 to 1\.5 \(beta\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-supported.html)  | 
| 1\.6 to 1\.7 \(beta\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-supported.html)  | 
|  1\.8 to 1\.14 \(beta\)   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-supported.html)  | 
| 1\.15 or later \(beta\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-supported.html)  | 