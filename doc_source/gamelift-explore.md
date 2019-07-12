# Explore Amazon GameLift<a name="gamelift-explore"></a>

Looking to experiment with Amazon GameLift features before diving in with your own game? Try out these sample experiences\. The console samples give you hands\-on experience with game hosting in the GameLift console\. The source code example and walkthrough shows you how to prepare a game for hosting using Realtime Servers\.

## Realtime Servers Sample Game \(Full Source\)<a name="gamelift-explore-buildsample"></a>

Mega Frog Race is a complete multiplayer game sample with source code\. Follow a hands\-on tutorial that walks through how to prepare the sample to run online using GameLift Realtime Servers\. This sample is a good way to better understand how to get your game client ready to work with GameLift Realtime Servers\. You can also use it as a starting point to experiment with other GameLift features such as FlexMatch\. 

To read the hands\-on tutorial, see the GameTech blog post [Creating Servers for Multiplayer Mobile Games with Just a Few Lines of JavaScript](http://aws.amazon.com/blogs/gametech/creating-servers-for-multiplayer-mobile-games-with-amazon-gamelift/)\. 

To get the source code, go to the [GitHub repository](https://github.com/aws-samples/megafrograce-gamelift-realtime-servers-sample)\.

The source material for the MegaFrogRace sample includes all the elements to deploy the multiplayer game hosted with Realtime Servers\.
+ Game client – Source code for the Unity\-created C\+\+ game client\. It illustrates how to get game session connection information from GameLift, connect to a Realtime server, and exchange game updates with other players via the Realtime server\. 
+ Client service – Source \(in Node\-based JavaScript\) for an AWS Lambda function that manages direct API calls to the GameLift service\. When called by the game client, the service makes requests to find or start new game sessions and assign players, and then returns connection details back to the game client\.
+ Realtime script – Source script file \(in Node\-based JavaScript\) that configures a fleet of Realtime servers for the game\. This script includes the minimum configuration to enable Realtime servers to communicate with the GameLift service and to start and stop game sessions\. It also includes some custom logic for the sample game\. 

## Custom Game Server Sample \(Console Experience\)<a name="gamelift-explore-deploycustomsample"></a>

This sample experience quickly gets you working with a live game on GameLift\. Upload a sample game build, create a fleet to run the game server, and connect to it from a sample game client\. You can start up multiple game clients and play to generate hosting data\. Once you have some data, explore the GameLift console to view your hosting resources, track metrics, and experiment with ways to scale hosting capacity\. 

To access the sample wizard, sign into the [GameLift console](https://console.aws.amazon.com/gamelift), open the Amazon GameLift menu, and select **Custom game server sample**\.

**About the sample game**  
The sample game is developed using the Amazon Lumberyard game engine\. To run the game client, you need a Windows 7 64\-bit system and 300 MB of space\. [See additional requirements\.](http://docs.aws.amazon.com/lumberyard/latest/userguide/setting-up-system-requirements.html) 