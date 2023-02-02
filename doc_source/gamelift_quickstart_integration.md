# Prepare your game for GameLift<a name="gamelift_quickstart_integration"></a>

This topic describes the steps for preparing your multiplayer game for integration with managed GameLift hosting\. To prepare your game, you must activate communication between it and GameLift\.

## Prepare your custom game server<a name="gamelift-integration"></a>

To start and stop game sessions, and to perform other tasks, a game server must be able to notify GameLift about its status\. To activate communication with GameLift, add code to your game server project\. For more information, see [Integrate games with custom game servers](integration-custom-intro.md)\.

1. **Prepare your custom game server for hosting on GameLift\.**
   + Get the [Amazon GameLift Server SDK](http://aws.amazon.com/gamelift/getting-started/#Developer_Resources_and_Documentation) and build it for your preferred programming language and game engine\.
   + Add code to your game server project to activate communication with GameLift\.

1. **Prepare your game client to connect to GameLift hosted game sessions\.**
   + Add the AWS SDK to your backend service and game client project\. For more information, see [Download Amazon GameLift SDKs for client services](gamelift-supported.md#gamelift-supported-clients)\.
   + Add functionality to retrieve information on game sessions, place new game sessions, and reserve space for players on a game session\.
   + \(Optional\) Use FlexMatch for player matchmaking\. For more information, see [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.

## Prepare your Realtime server<a name="realtime-plan"></a>

GameLift Realtime Servers provides a lightweight server solution that you can configure to fit your game\. A Realtime server provides the same benefits that GameLift offers to game servers, but with reduced game server customizability\.

**Create a Realtime script for hosting on GameLift\.**

Realtime scripts contain your server configuration and optional custom game logic\. Realtime servers are built to start and stop game sessions, accept player connections, and manage communication with GameLift and between players in a game\. There are also hooks for you to add custom server logic for your game\. Realtime servers use Node\.js and JavaScript\. For more information, see [Creating a Realtime script](realtime-script.md) and [Test your integration with GameLift](gamelift_quickstart_test.md)\.