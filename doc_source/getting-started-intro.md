# Getting started with Amazon GameLift<a name="getting-started-intro"></a>

We recommend that you try the following examples before you use GameLift for your own game\. The custom game server example gives you experience with game hosting in the GameLift console\. The Realtime Servers example shows you how to prepare a game for hosting using Realtime Servers\.

To get started with GameLift for your own game, see [GameLift managed hosting roadmap](gamelift-quickstart-intro.md)\.

## Custom game server example<a name="gamelift-explore-deploycustomsample"></a>

This example demonstrates a live custom game on GameLift\. The example walks you through the following steps:
+ Uploading an example game build\.
+ Creating a fleet to run the game server\.
+ Connecting to the game server from the example game client\.

After these steps, you can start up multiple game clients and play the game to generate hosting data\. Then, you can explore the GameLift console to view your hosting resources, track metrics, and experiment with ways to scale hosting capacity\.

**Computer requirements**
+ Windows 7 or newer
+ 64\-bit operating system
+ 300 MB of space

**To try the custom game server example**

1. Sign in to the [GameLift console](https://console.aws.amazon.com/gamelift)\.

1. If you see the new console, in the navigation pane, choose **Use the old console**\.

   If you don't see the navigation pane with that option, then you're in the old console\. Continue to the next step\.

1. In the old console, on the dashboard, do either of the following:
   + Expand **How to deploy a game with a custom game server**, and then choose **Try it Now\!**
   + Next to **Amazon GameLift**, choose **Dashboard**, and then choose **Custom game server sample**\.

   The console opens to the **Custom game server sample** page\.

## Realtime Servers example game<a name="gamelift-explore-buildsample"></a>

This example is a complete multiplayer game named Mega Frog Race, with source code included\. The example helps you understand how to integrate your game client with Realtime Servers\. You can also use this example game as a starting point to experiment with other GameLift features such as FlexMatch\.

For a hands\-on tutorial, see [Creating Servers for Multiplayer Mobile Games with Just a Few Lines of JavaScript](http://aws.amazon.com/blogs/gametech/creating-servers-for-multiplayer-mobile-games-with-amazon-gamelift/) on the AWS for Games Blog\.

For the source code of Mega Frog Race, see the [GitHub repository](https://github.com/aws-samples/megafrograce-gamelift-realtime-servers-sample)\.

The source code includes the following parts:
+ Game client – A source code for the C\+\+ game client, created in Unity\. The game client gets game session connection information, connects to the server, and exchanges updates with other players\.
+ Backend service – A source code for an AWS Lambda function that manages direct API calls to GameLift\.
+ Realtime script – A source script file that configures a fleet of Realtime Servers for the game\. This script includes the minimum configuration required for Realtime Servers to communicate with GameLift and to host games\.