# GameLift release notes<a name="release-notes"></a>

The GameLift release notes provide details about new features, updates, and fixes related to the service\. These may also involve changes to the following components:
+ AWS SDK GameLift API and CLI commands
+ GameLift Server SDK for use with managed GameLift and custom game servers
+ GameLift Client SDK for use with Realtime Servers
+ GameLift Console

## SDK versions<a name="release-notes-history"></a>

The following tabs list all GameLift releases with SDK versions\. There is no requirement to use comparable SDKs for your game server and client integrations\. However, earlier versions of one SDK may not fully support the latest features in another\. For more information about GameLift SDKs, see [GameLift SDKs](gamelift-supported.md)\.

**Note**  
If you're currently uploading a game server build or creating a build resource with the AWS SDK using Signature Version 2 for request authentication, you should move to using Signature Version 4\. Support is ending for Signature Version 2, as described in [AWS signature version 2 turned off for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingAWSSDK.html#UsingAWSSDK-sig2-deprecation)\.

### Current version<a name="release-notes-latest"></a>


****  

| Release | AWS SDK version | Server SDK version | Realtime client SDK version | GameLift local |  |  | C\# SDK | C\+\+ SDK | Unreal plugin |  |  | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| [2021\-06\-03](#release-notes-06032021) | [1\.8\.168](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.168) or later | 4\.0\.2 | 3\.4\.2 | 3\.4\.0 | 1\.2\.0 | 1\.0\.5 | 

### Previous versions<a name="release-notes-earlier"></a>


****  

| Release | AWS SDK version | Server SDK version | Realtime client SDK version | GameLift local |  |  | C\# SDK | C\+\+ SDK | Unreal plugin |  |  | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| [2021\-03\-23](#release-notes-03232021) | [1\.8\.168](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.168) or later | 4\.0\.2 | 3\.4\.1 | 3\.3\.3 | 1\.1\.0 | 1\.0\.5 | 
| [2021\-03\-16](#release-notes-03162021) | [1\.8\.163](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.163) or later | 4\.0\.2 | 3\.4\.1 | 3\.3\.3 | 1\.1\.0 | 1\.0\.5 | 
| [2021\-02\-09](#release-notes-02092021) | [1\.8\.139](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.139) or later | 4\.0\.2 | 3\.4\.1 | 3\.3\.3 | 1\.1\.0 | 1\.0\.5 | 
| [2020\-12\-22](#release-notes-12222020) | [1\.8\.95](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.195) or later | 4\.0\.2 | 3\.4\.1 | 3\.3\.3 | 1\.1\.0 | 1\.0\.5 | 
| [2020\-11\-24](#release-notes-11242020) | [1\.8\.95](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.95) or later | 4\.0\.2 | 3\.4\.1 | 3\.3\.2 | 1\.1\.0 | 1\.0\.5 | 
| [2020\-11\-11](#release-notes-11112020) | [1\.8\.36](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.36) or later | 4\.0\.2 | 3\.4\.1 | 3\.3\.2 | 1\.1\.0 | 1\.0\.5 | 
| [2020\-09\-17](#release-notes-09172020) | [1\.8\.36](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.36) or later | 4\.0\.1 | 3\.4\.1 | 3\.3\.2 | 1\.1\.0 | 1\.0\.5 | 
| [2020\-08\-27](#release-notes-04162020) | [1\.7\.310](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.310) or later | 4\.0\.0 | 3\.4\.0 | 3\.3\.1 | 1\.1\.0 | 1\.0\.5 | 
| [2020\-04\-16](#release-notes-04162020) | [1\.7\.310](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.310) or later | 4\.0\.0 | 3\.4\.0 | 3\.3\.1 | 1\.1\.0 | 1\.0\.5 | 
| [2020\-04\-02](#release-notes-04022020) | [1\.7\.310](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.310) or later | 3\.4\.0 | 3\.4\.0 |  | 1\.1\.0 | 1\.0\.0 | 
| [2019\-12\-19](#release-notes-12192019) | [1\.7\.249](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.249) or later | 3\.4\.0 | 3\.4\.0 |  | 1\.1\.0 | 1\.0\.0 | 
| [2019\-11\-14](#release-notes-11142019) | [1\.7\.210](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.210) or later | 3\.4\.0 | 3\.4\.0 |  | 1\.1\.0 | 1\.0\.0 | 
| [2019\-10\-24](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2019-10-24/) | [1\.7\.210](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.210) or later | 3\.4\.0 | 3\.4\.0 |  | 1\.1\.0 | 1\.0\.0 | 
| [2019\-09\-03](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2019-09-03/) | [1\.7\.175](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.175) or later | 3\.4\.0 | 3\.4\.0 |  | 1\.1\.0 | 1\.0\.0 | 
| [2019\-07\-09](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2019-07-09/) | [1\.7\.140](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.140) or later  | 3\.3\.0 | 3\.3\.0 |  | 1\.0\.0 | 1\.0\.0 | 
| [2019\-04\-25](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2019-04-25/) | [1\.7\.91](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.91) or later | 3\.3\.0 | 3\.3\.0 |  | 1\.0\.0 | 1\.0\.0 | 
| [2019\-03\-07](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2019-03-07/) | [1\.7\.65](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.65) or later | 3\.3\.0 | 3\.3\.0 |  |  | 1\.0\.0 | 
| [2019\-02\-07](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2019-02-07/) | [1\.7\.45](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.45) or later | 3\.3\.0 | 3\.3\.0 |  |  | 1\.0\.0 | 
| [2018\-12\-14](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-12-14/) | [1\.6\.20](https://github.com/aws/aws-sdk-cpp/releases/tag/1.6.20) or later | 3\.3\.0 | 3\.3\.0 |  |  | 1\.0\.0 | 
| [2018\-09\-27](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-09-27/) | [1\.6\.20](https://github.com/aws/aws-sdk-cpp/releases/tag/1.6.20) or later | 3\.2\.1 | 3\.2\.1 |  |  | 1\.0\.0 | 
| [2018\-06\-14](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-06-14/) | [1\.4\.47](https://github.com/aws/aws-sdk-cpp/releases/tag/1.4.47) or later | 3\.2\.1 | 3\.2\.1 |  |  | 1\.0\.0 | 
| [2018\-05\-10](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-05-10/) | [1\.4\.47](https://github.com/aws/aws-sdk-cpp/releases/tag/1.4.47) or later | 3\.2\.1 | 3\.2\.1 |  |  | 1\.0\.0 | 
| [2018\-02\-15](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-02-15/) | [1\.3\.58](https://github.com/aws/aws-sdk-cpp/releases/tag/1.3.58) or later | 3\.2\.1 | 3\.2\.1 |  |  | 1\.0\.0 | 
| [2018\-02\-08](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2018-02-08/) | [1\.3\.52](https://github.com/aws/aws-sdk-cpp/releases/tag/1.3.52) or later | 3\.2\.0 | 3\.2\.0 |  |  | 1\.0\.0 | 
| [2017\-09\-01](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2017-08-31/) | [1\.1\.43](https://github.com/aws/aws-sdk-cpp/releases/tag/1.1.43) or later | 3\.1\.7 | 3\.1\.7 |  |  | 1\.0\.0 | 
| [2017\-08\-16](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2017-08-16/) | [1\.1\.31](https://github.com/aws/aws-sdk-cpp/releases/tag/1.1.31) or later | 3\.1\.7 | 3\.1\.7 |  |  | 1\.0\.0 | 
| [2017\-05\-16](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2017-05-16/) | [1\.0\.122](https://github.com/aws/aws-sdk-cpp/releases/tag/1.0.122) or later | 3\.1\.5 | 3\.1\.5 |  |  | 1\.0\.0 | 
| [2017\-04\-11](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2017-04-11/) | [1\.0\.103](https://github.com/aws/aws-sdk-cpp/releases/tag/1.0.103) or later | 3\.1\.5 | 3\.1\.5 |  |  | 1\.0\.0 | 
| [2017\-02\-21](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2017-02-21/) | [1\.0\.72](https://github.com/aws/aws-sdk-cpp/releases/tag/1.0.72) or later | 3\.1\.5  | 3\.1\.5  |  |  |  | 
| [2016\-11\-18](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2016-11-18/) | [1\.0\.31](https://github.com/aws/aws-sdk-cpp/releases/tag/1.0.31) or later |  | 3\.1\.0 |  |  |  | 
| [2016\-10\-13](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2016-10-13/) | [1\.0\.17](https://github.com/aws/aws-sdk-cpp/releases/tag/1.0.17) or later |  | 3\.1\.0 |  |  |  | 
| [2016\-09\-01](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2016-09-01/) |  [0\.14\.9](https://github.com/aws/aws-sdk-cpp/releases/tag/0.14.9) or later |  | 3\.1\.0  |  |  |  | 
| [2016\-08\-04](https://aws.amazon.com/releasenotes/release-amazon-gamelift-on-2016-08-04/) |  [0\.12\.16](https://github.com/aws/aws-sdk-cpp/releases/tag/0.12.16) or later  |  | 3\.0\.7  |  |  |  | 

## Release notes<a name="release-notes-summary"></a>

The following release notes are in chronological order, with the latest updates listed first\. GameLift was first released in 2016\. For release notes dated earlier than those listed here, see the release date links in [SDK versions](#release-notes-history)\.

### February 15, 2022: FlexMatch adds compound rule and additional improvements<a name="release-notes-02152022"></a>

*GameLift releases new features for the FlexMatch matchmaking process\. *

**Release date:** February 15, 2022

FlexMatch users now have access to the following features:
+ **Compound rule** – Added support for compound matchmaking rules for matches of 40 or fewer players\. You can now use logical statements to create a compound rule to form a match\. Without a compound rule in your rule set, to form a match, all the rules in the rule set must be true\. With compound rules, you can choose which rules to apply using the following logical operators: `and`, `or`, `not`, and `xor`\.
+ **Flexible team selection** – Updated matchmaking property expressions to support selecting a subset of all available teams\.
+ **Longer string lists** – Increased the maximum number of strings from 10 to 100 in a list of strings of player attribute values\.

**Learn more:**
+ [GameLift FlexMatch developer guide](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide):
  + [FlexMatch rule types](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-rules-reference-ruletype.html)
  + [FlexMatch property expressions](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-rules-reference-property-expression.html)
+ [AttributeValue: SL](https://docs.aws.amazon.com/gamelift/latest/apireference/API_AttributeValue.html#gamelift-Type-AttributeValue-SL)

### October 28, 2021: GameLift adds support for multi\-Region fleets in the osaka Region; GameLift FleetIQ adds support for AWS Graviton2 processors<a name="release-notes-10282021"></a>

*Added GameLift support for multi\-Region fleets in the Osaka Region and GameLift FleetIQ support for AWS Graviton2 processors\.*

**Release date:** October 28, 2021

GameLift is now available in the Asia Pacific \(Osaka\) AWS Region\. Game developers can now deploy instances in Osaka using GameLift multi\-Region fleet\. 

You can now use Graviton2\-hosted game servers, based on the Arm\-based processor architecture, to achieve increased performance at a lower cost when compared to the equivalent Intel\-based compute options\.

GameLift FleetIQ instances powered by AWS Graviton2 processors include the c6g, m6g, and r6g instance families\. Available Regions are Asia Pacific \(Mumbai\), Asia Pacific \(Seoul\), Asia Pacific \(Singapore\), Asia Pacific \(Sydney\), Asia Pacific \(Tokyo\), Canada \(Central\), Europe \(Frankfurt\), Europe \(Ireland\), Europe \(London\), South America \(Sao Paulo\), US East \(N\. Virginia\), US East \(Ohio\), and US West \(N\. California\), US West \(Oregon\)\.
+ GameLift is now available in the Asia Pacific \(Osaka\) Region\.
+ GameLift FleetIQ game server groups can now be configured to manage the Graviton2 instance families c6g, m6g, and r6g\. Supported Amazon EC2 instance types, as listed for the GameServerGroup [InstanceDefinition](https://docs.aws.amazon.com/gamelift/latest/apireference/API_InstanceDefinition.html), now include the following:
  + c6g\.medium, c6g\.large, c6g\.xlarge, c6g\.2xlarge, c6g\.4xlarge, c6g\.8xlarge, c6g\.12xlarge, c6g\.16xlarge 
  + m6g\.medium, m6g\.large, m6g\.xlarge, m6g\.2xlarge, m6g\.4xlarge, m6g\.8xlarge, m6g\.12xlarge, m6g\.16xlarge
  + r6g\.medium, r6g\.large, r6g\.xlarge, r6g\.2xlarge, r6g\.4xlarge, r6g\.8xlarge, r6g\.12xlarge, r6g\.16xlarge

**Learn more:**
+ [GameLift multi\-Region fleet](https://aws.amazon.com/blogs/gametech/amazon-gamelift-is-now-easier-to-manage-fleets-across-regions)
+ [CreateGameServerGroup](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameServerGroup.html)
+ [AWS graviton processor](https://aws.amazon.com/ec2/graviton/)

### September 20, 2021: GameLift adds support for the Plug\-in for Unity<a name="release-notes-09202021"></a>

*Amazon GameLift now provides an Amazon GameLift Plug\-in for Unity that you can use to integrate GameLift into your Unity game\.*

**Release date:** September 20, 2021

The Amazon GameLift Unity Plug\-in version 1\.0\.0 contains libraries and native UI that makes it easier to access GameLift resources and integrate GameLift into your Unity game\. You can use the Amazon GameLift Plug\-in for Unity to access GameLift APIs and deploy AWS CloudFormation templates for common gaming scenarios\. The plugin also includes a sample game that works with the sample scenarios\. You can use GameLift Local to see messages passed between the game client and the game server to learn how a typical game interacts with GameLift\.

The Plug\-in for Unity supports Unity 2019\.4 LTS and 2020\.3 LTS\.

Highlights:
+ Learn the basics of integrating a game with GameLift with the sample game provided with the Plug\-in for Unity\. Build, run, and modify the sample with different scenarios, or create your own\. 
+ Deploy sample AWS CloudFormation scenarios for typical game scenarios including auth only, single\-Region fleet, multi\-Region fleets with queue and custom matchmaker, Spot Fleets with queue and custom matchmaker, and FlexMatch\.

**Learn more:**
+ [Integrating games with the Amazon GameLift Plug\-in for Unity](https://docs.aws.amazon.com/gamelift/latest/developerguide/unity-plugin.html)

### June 30, 2021: FlexMatch adds batchDistance rule<a name="release-notes-06302021"></a>

*Amazon GameLift now supports the new batchDistance rule type, making it easier to balance players by different attributes \(such as skill or maps\), create simpler syntax for common rules, and create expansions based on batch distance over time\.*

**Release date:** June 30, 2021

Added the batchDistance rule type, which you can use to specify a string or numeric attribute, bringing a host of benefits to each segment\.

When playing multiplayer games, players expect to enter sessions quickly and exit sessions with a sense of accomplishment\. To achieve this thrill for players, a good matchmaker must group together players based on their attributes, including skill level, playstyle, and latency\. Often times, players get grouped in either large matches \(>40 players\) or small matches \(up to 40\)\. The new batchDistance rule type can help\.

Highlights:
+ For large matches \(>40 players\), instead of evenly balancing players by skill only, you can now get that same balance based on skill, modes, and maps\. Ensure that everyone in the match is in a skill band, band multiple numeric attributes such as league or playstyle, and group according to string attributes such as map or game mode\. You can also create expansions over time\. For example, you can create an expansion to allow a greater skill level range to enter the match the longer the player is waiting\. 

  For matches under 40 players, you can use a new simplified rules expression\.

**Learn more:**
+ [GameLift FlexMatch developer guide](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/):
  + [Match rules reference](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-rules-reference.html)
  + [Rule set examples](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-examples.html)

### June 3, 2021: GameLift realtime client SDK and server SDK updates<a name="release-notes-06032021"></a>

*Amazon GameLift now supports IL2CPP in the RTS Client SDK, building the native libraries as frameworks, supports DescribePlayerSessions and GetInstanceCertificate in the Unreal Plugin, and adds Server SDK support for Unreal version 4\.26\.*

**Release date:** June 3, 2021

**Updated SDK versions:** AWS SDK [1\.8\.168](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.168)

With this latest SDK update, you can now integrate IL2CPP into your mobile applications that use the RTS Client SDK and follow best practices with frameworks\. You can also now build the Amazon GameLift Server SDK for Unreal Version 4\.26\. This update contains components that integrate with your Windows or Linux game server, including C\+\+ and C\# versions of the GameLift Server SDK, Amazon GameLift Local, and an Unreal Engine plugin\.

Highlights:
+ Added support for IL2CPP in the RTS Client SDK and for building the native libraries as frameworks, so you can build RTS clients for the latest mobile devices\.
+ You can use [DescribePlayerSessions\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-describeplayersessions) to get information for a single player session, for all player sessions in a game session, or for all player sessions associated with a single player ID\.
+ You can use [GetInstanceCertificate\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-getinstancecertificate) to retrieve the file location of a PEM\-encoded TLS certificate that is associated with the fleet and its instances\. 
+ Created Server SDK support for Unreal version 4\.26\.
+ The existing C\# SDK, version 4\.0\.2, has been verified compatible with Unity 2020\.3\. No SDK updates were required\.

**Learn more:**
+ [GameLift Developer Guide](https://docs.aws.amazon.com/gamelift/latest/developerguide/):
  + [DescribePlayerSessions\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-describeplayersessions) 
  + [GetInstanceCertificate\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-getinstancecertificate)

### March 23, 2021: GameLift adds notifications to game session placement<a name="release-notes-03232021"></a>

*Amazon GameLift now provides event notification for tracking game session placement requests\. *

**Release date:** March 23, 2021

**Updated SDK versions:** AWS SDK [1\.8\.168](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.168)

You can now use events to monitor game session placement activity for a game session queue\. Create an Amazon Simple Notification Service \(Amazon SNS\) topic to publish event notifications, or set up event tracking using CloudWatch Events\.

Highlights:
+ For each queue, you can set a custom text string to be included in all event messaging\.
+ When using an Amazon SNS topic, you can set additional access conditions that limit publishing to specific queues\.

**Learn more:**
+ [GameLift Developer Guide](https://docs.aws.amazon.com/gamelift/latest/developerguide/):
  + [Set up event notification for game session placement](queue-notification.md) \(new\)
  + [Game session placement events](queue-events.md) \(new\)
+ [API reference \(AWS SDK\)](https://docs.aws.amazon.com/gamelift/latest/developerguide/;reference-awssdk.html)
  + New game session queue parameters `NotificationTarget` and `CustomEventData`: [GameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionQueue.html), [CreateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSessionQueue.html), [UpdateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSessionQueue.html)
+ [GameLift forum](https://forums.awsgametech.com/c/amazon-gamelift/7)

### March 16, 2021: GameLift adds multi\-Region fleets, six new regions<a name="release-notes-03162021"></a>

*Amazon GameLift broadens global coverage for game hosting, adding six new AWS Regions and introducing multi\-Region fleets, which can manage game server deployment across multiple locations\.*

**Release date:** March 16, 2021

**Updated SDK versions:** AWS SDK [1\.8\.163](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.163)

GameLift managed hosting is now available in 21 AWS Regions\. The new Regions are Cape Town \(`af-south-1`\), Bahrain \(`me-south-1`\), Hong Kong \(`ap-east-1`\), Milan \(`eu-south-1`\), Paris \(`eu-west-3`\), and Stockholm \(`eu-north-1`\)\.

With the new GameLift multi\-location fleets feature, you can now set up a single fleet to host your game servers in any or all of 20 GameLift\-supported Regions \(Beijing Region excepted\)\. This feature aims to significantly reduce the work required to set up and maintain GameLift hosting resources globally\. Multi\-location fleets can be created in the following AWS Regions: `us-east-1` \(N\. Virginia\), `us-west-2` \(Oregon\), `eu-central-1` \(Frankfurt\), `eu-west-1` \(Ireland\), `ap-southeast-2` \(Sydney\), `ap-northeast-1` \(Tokyo\), and `ap-northeast-2` \(Seoul\)\. In all other Regions, you can continue to set up single\-location fleets as needed\. All fleets that were created before this release are single\-location fleets\. Using multi\-location fleets does not affect your hosting costs\. GameLift pricing is based on the type, location, and volume of instances that you use\. \(For more information, see [GameLift pricing](http://aws.amazon.com/gamelift/pricing/)\.\) AWS CloudFormation support for multi\-location fleets will be available soon\.

**Note**  
Multi\-location fleets are not available in the China Region\. GameLift resources that reside in a China Region cannot interact with or be used by resources in other GameLift Regions\.

Highlights: 
+ With a multi\-location fleet, explicitly add a list of remote locations\. GameLift deploys instances of the same type and configuration, including the build and runtime configuration, to the fleet's home Region and all added locations\. 
+ Adjust capacity settings and scaling for each location independently\. Auto scaling policies apply to an entire fleet, but you can turn them on or off by location\.
+ Start new game sessions at specific fleet locations\. When using game session queues or matchmaking to place game sessions, you can now prioritize where new game sessions start by location, hosting cost, and player latency\.
+ Get hosting metrics in the GameLift console, aggregated for all locations in a fleet or broken out by each fleet location\. 

**Learn more:**
+ [Amazon game tech blog](https://aws.amazon.com/blogs/gametech/)
+ [API reference \(AWS SDK\)](https://docs.aws.amazon.com/gamelift/latest/developerguide/;reference-awssdk.html)
  + New fleet location operations: [CreateFleetLocations](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleetLocations.html), [DescribeFleetLocationAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetLocationAttributes.html), [DescribeFleetLocationCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetLocationCapacity.html), [DescribeFleetLocationUtilization](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetLocationUtilization.html), [DeleteFleetLocations](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DeleteFleetLocations.html)
  + Updated fleet operations, with new multi\-location support: [CreateFleet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html), [UpdateFleetCapacity](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetCapacity.html), [DescribeEC2InstanceLimits](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeEC2InstanceLimits.html), [DescribeInstances](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeInstances.html), [StopFleetActions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopFleetActions.html), [StartFleetActions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartFleetActions.html)
  + Updated game session placement operations, with new priority and filtering capability: [CreateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSessionQueue.html), [DescribeGameSessionQueues](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionQueues.html), [UpdateGameSessionQueue](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateGameSessionQueue.html)
  + Updated game session creation operations, with new location support: [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html), [DescribeGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessions.html), [DescribeGameSessionDetails](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionDetails.html), [SearchGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_SearchGameSessions.html)
+ [GameLift Developer Guide](https://docs.aws.amazon.com/gamelift/latest/developerguide/;):
  + [Using GameLift in AWS regions](gamelift-regions.md) \(updated\)
  + [GameLift fleet design guide](fleets-design.md) \(new\)

    [Scaling GameLift hosting capacity](fleets-manage-capacity.md) \(updated\)
  + [Design a game session queue](queues-design.md) \(new\)
  + [View fleet details](gamelift-console-fleets-metrics.md) \(updated\)
+ [GameLift forum](https://forums.awsgametech.com/c/amazon-gamelift/7)

### February 9, 2021: GameLift extends support for AMD instances, standalone FlexMatch<a name="release-notes-02092021"></a>

**Customers in China \(Beijing\) and all GameLift FleetIQ customers can now use AMD instances\. GameLift FlexMatch as a standalone matchmaking solution is now available in China \(Beijing\), and FlexMatch matchmaking notification now supports Amazon SNS FIFO topics\.**

**Release date:** February 9, 2021

**Updated SDK versions:** AWS SDK [1\.8\.139](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.139)

This release includes the following updates:
+  GameLift FleetIQ game server groups can now be configured to manage the AMD instance families C5a, M5a, and R5a\. The supported Amazon EC2 instance types, as listed for the GameServerGroup [InstanceDefinition](https://docs.aws.amazon.com/gamelift/latest/apireference/API_InstanceDefinition.html), now include the following:
  + c5a\.large, c5a\.xlarge, c5a\.2xlarge, c5a\.4xlarge, c5a\.8xlarge, c5a\.12xlarge, c5a\.16xlarge, c5a\.24xlarge 
  + m5a\.large, m5a\.xlarge, m5a\.2xlarge, m5a\.4xlarge, m5a\.8xlarge, m5a\.12xlarge, m5a\.16xlarge, m5a\.24xlarge 
  + r5a\.large, r5a\.xlarge, r5a\.2xlarge, r5a\.4xlarge, r5a\.8xlarge, r5a\.12xlarge, r5a\.16xlarge, r5a\.24xlarge 

  Note: AMD instances for FleetIQ are currently not available for use in the China \(Beijing\) AWS Region\. See [Feature availability and implementation differences](https://docs.amazonaws.cn/en_us/aws/latest/userguide/gamelift.html) in China\.
+ GameLift managed game hosting now supports AMD instances in the China \(Beijing\) Region, operated by Sinnet\. The new AMD instance families include M5a and R5a\. Supported EC2 instance types, as listed for fleet [InstanceType](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetAttributes.html), now include the following:
  + m5a\.large, m5a\.xlarge, m5a\.2xlarge, m5a\.4xlarge, m5a\.8xlarge, m5a\.12xlarge, m5a\.16xlarge, m5a\.24xlarge 
  + r5a\.large, r5a\.xlarge, r5a\.2xlarge, r5a\.4xlarge, r5a\.8xlarge, r5a\.12xlarge, r5a\.16xlarge, r5a\.24xlarge 
+ GameLift FlexMatch can now be used as a standalone matchmaking solution in the China \(Beijing\) Region, operated by Sinnet\. Customers can create a FlexMatch matchmaker in the Beijing Region and configure the [FlexMatchMode](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateMatchmakingConfiguration.html#gamelift-CreateMatchmakingConfiguration-request-FlexMatchMode) parameter to STANDALONE\. For more information about FlexMatch, either with GameLift managed hosting or with a non\-GameLift hosting solution, in the [GameLift FlexMatch Developer Guide](https://docs.amazonaws.cn/en_us/gamelift/latest/flexmatchguide/match-intro.html)\.
+ When setting up event notifications for GameLift FlexMatch, you can now designate an Amazon SNS FIFO topic as the notification target\. For more information, see:
  + [MatchmakingConfiguration NotificationTarget](https://docs.aws.amazon.com/gamelift/latest/apireference/API_MatchmakingConfiguration.html), *Amazon GameLift API Reference*
  + [ Set up FlexMatch event notification ](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-notification.html), *GameLift FlexMatch Developer Guide*
  + [ Introducing Amazon SNS FIFO – First\-in\-first\-out Pub/Sub messaging](https://aws.amazon.com/blogs/aws/introducing-amazon-sns-fifo-first-in-first-out-pub-sub-messaging/), *AWS News Blog*

### December 22, 2020: GameLift server SDK supports unreal engine 4\.25 and unity 2020<a name="release-notes-12222020"></a>

*Amazon GameLift releases an updated Server SDK to support game server development with Unreal Engine 4\.25 and verifies support for Unity 2020\.*

**Release date:** December 22, 2020

**Updated SDK versions:** GameLift Server SDK 4\.0\.2, Unreal plugin version 3\.3\.3

The latest version of the GameLift Server SDK contains the following components:
+ The updated Unreal plugin has been updated for compatibility with Unreal Engine 4\.25\. The API was not changed\.
+ The existing C\# SDK, version 4\.0\.2, has been verified compatible with Unity 2020\. No SDK updates were required\.

Download the latest version of the GameLift Server SDK at [ Amazon GameLift getting started](https://aws.amazon.com/gamelift/getting-started)\.

### November 24, 2020: GameLift FlexMatch now available for games hosted anywhere<a name="release-notes-11242020"></a>

*Amazon GameLift extends availability of the FlexMatch matchmaking service to multiplayer games that use any hosting solution, including peer\-to\-peer, on\-premises, or cloud services\.*

**Release date:** November 24, 2020

**Updated SDK versions:** AWS SDK [1\.8\.95](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.95)

GameLift FlexMatch is a customizable matchmaking service for multiplayer games\. Initially designed for users of GameLift managed hosting, FlexMatch can now be integrated into games that use other hosting systems, including peer\-to\-peer, proprietary on\-premises computing, and cloud compute primitive types\. Games that use GameLift FleetIQ for game hosting on Amazon EC2 can now implement matchmaking with FlexMatch\.

FlexMatch provides a robust matchmaking algorithm and rules language that gives you wide latitude to customize the matchmaking process so that players are matched together based on key player characteristics and reported latency\. In addition, FlexMatch offers a matchmaking request workflow that supports features such as player parties, player acceptance, and match backfill\. When you use FlexMatch with GameLift managed hosting or Realtime Servers, the matchmaker automatically uses GameLift to find hosting resources and start a new game session for newly formed matches\. When using FlexMatch as a standalone service, the matchmaker delivers match results back to your game, which can then start a new game session using your hosting solution\.

API operations for FlexMatch are part of the GameLift service API, which is included in the AWS SDK and the AWS Command Line Interface \(AWS CLI\)\. This release includes these updates to support standalone matchmaking:
+ The API resource `MatchmakingConfiguration` has the following changes: 
  + New property, `FlexMatchMode` indicates whether the matchmaker is being used with GameLift managed hosting or as standalone matchmaking\.
  + Property `GameSessionQueueArns` is not required when `FlexMatchMode` is set to standalone\.
  + These properties are not used with standalone matchmaking: `AdditionalPlayerCount`, `BackfillMode`, `GameProperties`, `GameSessionData`\.
+ The automatic backfill feature is not available with standalone matchmaking\.

**Learn more:**
+ [Amazon game tech blog](https://aws.amazon.com/blogs/gametech/)
+ [GameLift FlexMatch developer guide](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/):
  + [How GameLift FlexMatch works](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/gamelift-match.html)
  + [Getting started with FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-getting-started.html)
  + [FlexMatch API reference \(AWS SDK\)](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/reference-awssdk-flex.html)
+ [GameLift forum](https://forums.awsgametech.com/c/amazon-gamelift/7)

**Note**  
The GameLift documentation has been expanded\. To find all GameLift content and resources, see the GameLift documentation home page\. GameLift developer documentation now includes:  
[GameLift Developer Guide](https://docs.aws.amazon.com/gamelift/latest/developerguide/) for use with GameLift managed hosting and Realtime Servers\.
[GameLift FleetIQ Developer Guide](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/) to optimize use of Amazon EC2 Spot Instances for game hosting\.
[GameLift FlexMatch Developer Guide](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/) for matchmaking\.

### November 24, 2020: AMD instances now available on GameLift<a name="release-notes-11242020-2"></a>

*Amazon GameLift now offers AMD instance families for additional cost savings with multiplayer game hosting*\.

**Release date:** November 24, 2020

**Updated SDK versions:** AWS SDK [1\.8\.95](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.95)

The list of Amazon EC2 instance types supported by GameLift now includes three new instance families: C5a, M5a, and R5a\. These families consist of AMD compute\-optimized instances that are powered by AMD EPYC processors running at frequencies up to 3\.3\. GHz\. The AMD instances are x86 compatible; games that are currently running on GameLift can be deployed to AMD instance types without alteration\. The new instances are available in the following AWS Regions: US East \(N\. Virginia and Ohio\), US West \(Oregon and N\. California\), Central Canada \(Montreal\), South America \(Sao Paulo\), EU Central \(Frankfurt\), EU West \(London and Ireland\), Asia Pacific South \(Mumbai\), Asia Pacific Northeast \(Seoul and Tokyo\), and Asia Pacific Southeast \(Singapore and Sydney\)\.

The new AMD instances include: 
+ c5a\.large, c5a\.xlarge, c5a\.2xlarge, c5a\.4xlarge, c5a\.8xlarge, c5a\.12xlarge, c5a\.16xlarge, c5a\.24xlarge
+ m5a\.large, m5a\.xlarge, m5a\.2xlarge, m5a\.4xlarge, m5a\.8xlarge, m5a\.12xlarge, m5a\.16xlarge, m5a\.24xlarge
+ r5a\.large, r5a\.xlarge, r5a\.2xlarge, r5a\.4xlarge, r5a\.8xlarge, r5a\.12xlarge, r5a\.16xlarge, r5a\.24xlarge

**Learn more:**
+ [Amazon game tech blog](https://aws.amazon.com/blogs/gametech/)
+ [Amazon GameLift instance pricing](https://aws.amazon.com/gamelift/pricing)
+ [Amazon EC2 instances featuring AMD EPYC processors](https://aws.amazon.com/ec2/amd/)
+ [GameLift forum](https://forums.awsgametech.com/c/amazon-gamelift/7)

### November 11, 2020: Version update to GameLift server SDK<a name="release-notes-11112020"></a>

*Amazon GameLift releases updated Server SDK to fix issue with `StartMatchBackfill()`*

**Release date:** November 11, 2020

**Updated SDK versions:** GameLift Server SDK 4\.0\.2

The new Server SDK version 4\.0\.2 fixes a known issue with the API operation `StartMatchBackfill()`\. This operation now returns a correct response to a match backfill request\. 

The issue did not affect the match backfill process, and there is no change to how this feature works\. The issue may have impacted log messaging and error handling for match backfill requests\.

Download the latest version of the GameLift Server SDK at [ Amazon GameLift getting started](https://aws.amazon.com/gamelift/getting-started)\.

### November 5, 2020: New FlexMatch algorithm customizations<a name="release-notes-11052020"></a>

*Amazon GameLift releases customization options for FlexMatch matchmaking process\.*

**Release date:** November 5, 2020

FlexMatch users can now adjust the following default behaviors for the matchmaking process\. These customizations are set in a matchmaking rule set\. There are no changes to the GameLift SDKs\.
+ Prioritize backfill tickets: You can choose to raise or lower how match backfill tickets are prioritized when searching for acceptable matches\. Prioritizing backfill tickets is useful when the auto\-backfill feature is enabled\. Use the algorithm property `backfillPriority`\.
+ Pre\-sort to optimize match consistency and efficiency: Configure your matchmaker to pre\-sort the ticket pool before batching tickets for evaluation\. By pre\-sorting tickets based on key player attributes, your resulting matches tend to have players who are more similar in those attributes\. You can also boost efficiency in the evaluation process by pre\-sorting on the same attributes that are used in match rules\. Use the algorithm property `sortByAttributes` with the `strategy` property set to "sorted"\. 
+ Adjust how expansion wait times are triggered: Choose between triggering expansions based on the age of the newest \(default\) or oldest ticket in an incomplete match\. Triggering on the oldest ticket tends to complete matches faster, while triggering on the newest ticket leads to higher match quality\. Use the algorithm property `expansionAgeSelection`\.

**Learn more:**

GameLift Developer Guide
+ [Design a FlexMatch rule set: Customize the match algorithm](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-design-ruleset.html#match-rulesets-components-algorithm)
+ [Rule set schema](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-ruleset-schema.html) – This topic now has detailed reference information for each rule set component\.

### September 17, 2020: GameLift updates server SDK<a name="release-notes-09172020"></a>

*Amazon GameLift releases minor updates to the Server SDK*\.

**Release date:** September 17, 2020

**Updated SDK versions:** GameLift Server SDK 4\.0\.1

The new Server SDK contains the following updates:
+ C\# API version 4\.0\.1
  + The API operation [TerminateGameSession\(\)](integration-server-sdk-csharp-ref-actions.md#integration-server-sdk-csharp-ref-terminategamesession) is no longer supported\. Replace with a call to [ProcessEnding\(\)](integration-server-sdk-csharp-ref-actions.md#integration-server-sdk-csharp-ref-processending) to end both a game session and the server process\.
  + A known issue with the operation [GetInstanceCertificate\(\)](integration-server-sdk-csharp-ref-actions.md#integration-server-sdk-csharp-ref-getinstancecertificate) is fixed\.
  + The operation [GetTerminationTime\(\)](integration-server-sdk-csharp-ref-actions.md#integration-server-sdk-csharp-ref-getterm) now returns a value of data type AwsDateTimeOutcome\.
+ C\+\+ API version 3\.4\.1
  + The operation [TerminateGameSession\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-terminategamesession) is no longer supported\. Replace it with a call to [ProcessEnding\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processending) to end both a game session and the server process\.
+ Unreal Engine plugin version 3\.3\.2
  + The operation [TerminateGameSession\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-terminategamesession) is no longer supported\. Replace it with a call to [ProcessEnding\(\)](integration-server-sdk-unreal-ref-actions.md#integration-server-sdk-unreal-ref-processending) to end both a game session and the server process\.
  + The callback operation `OnUpdateGameSession` is added to [FProcessParameters](integration-server-sdk-unreal-ref-datatypes.md#integration-server-sdk-unreal-ref-dataypes-process) to support match backfill\.

Download the latest version of the GameLift Server SDK at [ Amazon GameLift getting started](https://aws.amazon.com/gamelift/getting-started)\.

### August 27, 2020: GameLift FleetIQ for game hosting with Amazon EC2 \(general availability\)<a name="release-notes-08272020"></a>

*Amazon GameLift extends its FleetIQ for Spot Instance optimization\.*

**Release date:** August 27, 2020

**Updated SDK versions:** AWS SDK [1\.8\.36](https://github.com/aws/aws-sdk-cpp/releases/tag/1.8.36)

The GameLift FleetIQ solution for low\-cost, cloud\-based game hosting on Amazon EC2 is now generally available\. GameLift FleetIQ gives developers the ability to host game servers directly on Amazon EC2 Spot Instances by optimizing their viability for game hosting\. Game developers can use GameLift FleetIQ with new games or to supplement capacity for existing games\. This solution supports the use of containers or other AWS services such as AWS Shield and Amazon Elastic Container Service \(Amazon ECS\)\. 

This general availability release includes the following updates to the GameLift FleetIQ solution:
+ New API operation `DescribeGameServerInstances` returns information, including status, on all active instances for a GameLift FleetIQ game server group\.
+ New balancing strategy, `ON_DEMAND_ONLY`, configures a game server group to use On\-Demand Instances only\. You can update a game server group's balancing strategy at any time, making it possible to switch between using Spot Instances and On\-Demand Instances as needed\.
+ The following preview elements have been dropped for general availability:
  + Use of custom sort keys for game server resources\. Game servers can be sorted based on registration timestamp\.
  + Tagging for game server resources\.

**Learn more:** 
+ AWS Training and Certification course: [ Using Amazon GameLift FleetIQ for game servers](https://www.aws.training/LearningLibrary?filters=classification%3A59)
+ [GameLift FleetIQ API reference](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameServerGroup.html)
+ [GameLift FleetIQ developer guide](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-intro.html)
+ Announcements on [Amazon game tech blog](https://aws.amazon.com/blogs/gametech/) and [AWS what's new feed](https://aws.amazon.com/new/?awsf.whats-new-gametech=general-products%23amazon-gamelift)

### April 16, 2020: GameLift updates server SDK for unity and unreal engine<a name="release-notes-04022020"></a>

*Amazon GameLift releases an updated Server SDK to support game server development with Unity 2019 and Unreal Engine 4\.24\.*

**Release date:** April 16, 2020

**Updated SDK versions:** GameLift Server SDK 4\.0\.0, GameLift Local 1\.0\.5

The latest version of the GameLift Server SDK contains the following updated components:
+ C\# SDK version 4\.0\.0 updated for Unity 2019\.
+ Unreal plugin version 3\.3\.1 updated for Unreal Engine versions 4\.22, 4\.23, and 4\.24\.
+ GameLift Local version 1\.0\.5 updated to test integrations that use the C\# server SDK version 4\.0\.0\.

Download the latest version of the GameLift Server SDK at [ Amazon GameLift getting started](https://aws.amazon.com/gamelift/getting-started)\.

### April 2, 2020: GameLift FleetIQ available for game hosting on EC2 \(public preview\)<a name="release-notes-04162020"></a>

*Amazon GameLift extends its FleetIQ for Spot Instance optimization\.*

**Release date:** April 02, 2020

**Updated SDK versions:** AWS SDK [1\.7\.310](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.310)

The GameLift FleetIQ feature optimizes the viability of low\-cost Spot Instances for use with game hosting\. This feature is now extended for customers who want to manage their hosting resources directly rather than through the managed GameLift service\. This solution supports the use of containers or other AWS services such as AWS Shield and Amazon Elastic Container Service \(Amazon ECS\)\.

**Learn more:**

[GameTech blog post](https://aws.amazon.com/blogs/gametech/gamelift-in-2020-major-update-now-available-in-preview/) on GameLift FleetIQ

### December 19, 2019: Improved AWS resource management for GameLift resources<a name="release-notes-12192019"></a>

*Amazon GameLift adds new ARN values and tagging support\.*

**Release date:** December 19, 2019

**Updated SDK versions:** AWS SDK [1\.7\.249](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.249)

You can now take advantage of AWS resource management tools with GameLift resources\. In particular, all key GameLift resources—builds, scripts, fleets, game session queues, matchmaking configurations, and matchmaking rule sets—are now assigned Amazon Resource Name \(ARN\) values\. A resource ARN provides a consistent identifier that is unique across all AWS Regions\. They can be used to create resource\-specific AWS Identity and Access Management \(IAM\) permissions policies\. Resources are now assigned an ARN and also the pre\-existing resource identifier, which is not Region\-specific\. 

In addition, GameLift resources now support tagging\. You can use tags to organize resources, create IAM permissions policies to manage access to groups of resources, customize AWS cost breakdowns, etc\. When managing tags for GameLift resources, use the GameLift API actions `TagResource()`, `UntagResource()`, and `ListTagsForResource()`\.

**Learn more:**
+ [TagResource](https://docs.aws.amazon.com/gamelift/latest/apireference/API_TagResource.html) in the *Amazon GameLift API Reference* 
+ [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*
+ [ Amazon resource names](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *AWS General Reference*

### November 14, 2019: New AWS CloudFormation templates, updates in china \(beijing\) Region<a name="release-notes-11142019"></a>

*Amazon GameLift expands use of AWS CloudFormation templates to help automate GameLift resource management\.*

**Release date:** December 19, 2019

**Updated SDK versions:** AWS SDK [1\.7\.210](https://github.com/aws/aws-sdk-cpp/releases/tag/1.7.210)

AWS CloudFormation templates for GameLift

GameLift resources can now be created and managed through AWS CloudFormation\. The existing AWS CloudFormation build and fleet templates have been updated to align with the current resources, and new templates are now available for scripts, queues, matchmaking configurations, and matchmaking rule sets\. AWS CloudFormation templates greatly simplify the task of managing groups of related AWS resources, particularly when deploying games across multiple Regions\. 

**Learn more:**
+ [Amazon GameLift resource type reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_GameLift.html) in the *AWS CloudFormation User Guide*
+ [Create resources using AWS CloudFormation](resources-cloudformation.md) in the *Amazon GameLift Developer Guide*

## Lumberyard compatibility<a name="gamelift-supported-compatibility"></a>

For Amazon Lumberyard users, the following table lists the GameLift SDK versions that are bundled into or are compatible with the Lumberyard game engine\. 


****  

| Amazon Lumberyard version | Are bundled with GameLift SDK versions | 
| --- | --- | 
| 1\.4 to 1\.5 \(beta\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/release-notes.html)  | 
| 1\.6 to 1\.7 \(beta\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/release-notes.html)  | 
|  1\.8 to 1\.14 \(beta\)   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/release-notes.html)  | 
| 1\.15 to 1\.25 |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/release-notes.html)  | 
| 1\.26 and later |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/release-notes.html)  | 