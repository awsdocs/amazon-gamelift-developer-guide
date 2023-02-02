# Test your custom server integration<a name="integration-testing"></a>

Use Amazon GameLift to integrate hardware anywhere in your environment into your GameLift game hosting\. GameLift Anywhere registers your hardware with GameLift in an Anywhere fleet\. You can integrate Anywhere and Amazon Elastic Compute Cloud \(Amazon EC2\) fleets in matchmaker and game session queues to manage matchmaking and game placement across your game servers from GameLift\. You can iteratively test and build your game server project using your own hardware\. 

To use GameLift Anywhere download and integrate the GameLift Server SDK version 5 or greater\.

**Topics**
+ [Initial development](#fleet-anywhere-dev)
+ [Iterate on your game server](#fleet-anywhere-iteration)

## Initial development<a name="fleet-anywhere-dev"></a>

You've developed your game and you're ready to integrate with GameLift\. Creating an Amazon EC2 fleet and iterating on the game server code is slow due to uploading builds and creating new fleets for each iteration\. Using an Anywhere fleet on your development laptop makes GameLift and game server code iteration faster\.

Use the following procedure to create an Anywhere fleet and start a game session on your laptop using the GameLift console or the AWS Command Line Interface \(AWS CLI\)\.

------
#### [ Console ]

1. Open the [GameLift console](https://console.aws.amazon.com/gamelift)\.

1. In the navigation pane, under **Hosting**, choose **Locations**\.

1. Choose **Create location**\.

1. In the **Create location** dialog box, do the following:

   1. Enter a **Location name**\. This labels the location of your compute resources that GameLift uses to run your games in Anywhere fleets\. Custom location names must start with **custom\-**\.

   1. Choose **Create**\.

1. To create an Anywhere fleet, do the following:

   1. In the navigation pane, under **Hosting**, choose **Fleets**\.

   1. On the **Fleets** page, choose **Create fleet**\.

   1. On the **Choose compute type** step, choose **Anywhere**, and then choose **Next**\.

   1. On the **Define fleet details** step, define your new fleet\. For more information, see [Create a new GameLift fleet](fleets-creating-all.md)\.

   1. On the **Select locations** step, select the custom location that you created\.

   1. Complete the remaining fleet creation steps to create your Anywhere fleet\.

1. Register your laptop as a compute resource in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html) API operation\)\. Include the `fleet-id` created in the previous step and add a `compute-name` and your laptop's `ip-address`\.

   ```
   aws gamelift register-compute \
       --compute-name DevLaptop \
       --fleet-id fleet-1234 \
       --ip-address 10.1.2.3 \
       --location custom-location-1
   ```

   Example output:

   ```
   Compute {
       FleetId = fleet-1234,
       ComputeName = DevLaptop,
       Status = ACTIVE,
       IpAddress = 10.1.2.3,
       GameLiftServiceSdkEndpoint = wss://12345678.execute-api.amazonaws.com/
       Location = custom-location
   }
   ```

1. Start a debug session of your game server\.

   1. Get the authorization token for your laptop in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html) API operation\)\.

      ```
      aws gamelift get-compute-auth-token \
          --fleet-id fleet-1234 \
          --compute-name DevLaptop
      ```

      Example output:

      ```
      ComputeAuthToken {
        FleetId = fleet-1234,
        ComputeName = DevLaptop,
        AuthToken = abcdefg123,
        ExpirationTime = 1897492857.11
      }
      ```

   1. Run a debug instance of your game server executable\. To run the debug instance, your game server must call `InitSDK()`\. After the process is ready to host a game session, the game server calls `ProcessReady()`\.

1. Create a game session to test out your first integration with GameLift Anywhere\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html) API operation\)\.

   ```
   aws gamelift create-game-session \
       --fleet-id fleet-1234 \
       --name DebugSession \
       --maximum-player-session-count 2
   ```

   Example output:

   ```
   GameSession {
       FleetId = fleet-1234,
       GameSessionId = 1111-1111,
       Name = DebugSession,
       IpAddress = 10.1.2.3,
       Port = 1024,
       ...
   }
   ```

   GameLift sends an `onCreateGameSession()` message to your registered server process\. The message contains the `GameSession` object from the previous step with game properties, game sessions data, matchmaker data, and more about the game session\.

1. Add logic to your game server so that your server process responds to the `onCreateGameSession()` message with `ActivateGameSession()`\. The operation sends an acknowledgement to GameLift that your server received and accepted the create game session message\. For more information see, [Amazon GameLift server SDK reference](reference-serversdk.md)\.

Your game server is now running a game session for you to test out and use for iteration\. To learn how to iterate on your game server, continue to the next section\.

------
#### [ AWS CLI ]

1. Create a custom location using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-location.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-location.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateLocation.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateLocation.html) API operation\)\. A custom location labels the location of your hardware that GameLift uses to run your games in Anywhere fleets\.

   ```
   aws gamelift create-location \
       --location-name custom-location-1
   ```

   Example output:

   ```
   {
       Location {
           LocationName = custom-location-1
       }
   }
   ```

1. Create an Anywhere fleet with your custom location using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-fleet.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-fleet.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateFleet.html) API operation\)\. GameLift creates the fleet in your home Region and the custom locations that you provide\.

   ```
   aws gamelift create-fleet \
       --name LaptopFleet \
       --compute-type ANYWHERE \
       --locations "location=custom-location-1"
   ```

   Example output:

   ```
   Fleet {
       Name = LaptopFleet,
       ComputeType = ANYWHERE,
       FleetId = fleet-1234,
       Status = ACTIVE
       ...
   }
   ```

1. Register your laptop as a compute resource in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html) API operation\)\. Include the `fleet-id` created in the previous step and add a `compute-name` and your laptop's public `ip-address`\.

   ```
   aws gamelift register-compute \
       --compute-name DevLaptop \
       --fleet-id fleet-1234 \
       --ip-address 10.1.2.3 \
       --location custom-location-1
   ```

   Example output:

   ```
   Compute {
       FleetId = fleet-1234,
       ComputeName = DevLaptop,
       Status = ACTIVE,
       IpAddress = 10.1.2.3,
       GameLiftServiceSdkEndpoint = wss://12345678.execute-api.amazonaws.com/
       Location = custom-location-1
   }
   ```

1. Start a debug session of your game server\.

   1. Get the authorization token for your laptop in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html) API operation\)\.

      ```
      aws gamelift get-compute-auth-token \
          --fleet-id fleet-1234 \
          --compute-name DevLaptop
      ```

      Example output:

      ```
      ComputeAuthToken {
        FleetId = fleet-1234,
        ComputeName = DevLaptop,
        AuthToken = abcdefg123,
        ExpirationTime = 1897492857.11
      }
      ```

   1. Run a debug instance of your game server executable\. To run the debug instance, your game server must call `InitSDK()`\. After the process is ready to host a game session, the game server calls `ProcessReady()`\.

1. Create a game session to test out your first integration with GameLift Anywhere\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html) API operation\)\.

   ```
   aws gamelift create-game-session \
       --fleet-id fleet-1234 \
       --name DebugSession \
       --maximum-player-session-count 2
   ```

   Example output:

   ```
   GameSession {
       FleetId = fleet-1234,
       GameSessionId = 1111-1111,
       Name = DebugSession,
       IpAddress = 10.1.2.3,
       Port = 1024,
       ...
   }
   ```

   GameLift sends an `onCreateGameSession()` message to your registered server process\. The message contains the `GameSession` object from the previous step with game properties, game sessions data, matchmaker data, and more about the game session\.

1. Add logic to your game server so that your server process responds to the `onCreateGameSession()` message with `ActivateGameSession()`\. The operation sends an acknowledgement to GameLift that your server received and accepted the create game session message\. For more information see, [Amazon GameLift server SDK reference](reference-serversdk.md)\.

Your game server is now running a game session for you to test out and use for iteration\. To learn how to iterate on your game server, continue to the next section\.

------

## Iterate on your game server<a name="fleet-anywhere-iteration"></a>

In this use case, consider a scenario where you've set up and tested your game server and found a bug\. With GameLift Anywhere, you can iterate on your code and avoid the heavy setup of using an Amazon EC2 fleet\.

1. Clean up your existing `GameSession`, if possible\. If the game server crashes or it won't call `ProcessEnding()`, GameLift cleans up the `GameSession` after the game server stops sending health checks\.

1. Make the code changes to your game server, compile, and prepare for the next test\.

1. Your previous Anywhere fleet is still active and your laptop is still registered as a compute resource in the fleet\. To begin testing again, create a new debug instance\.

   1. Retrieve the authorization token for your laptop in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetComputeAuthToken.html) API operation\)\.

      ```
      aws gamelift get-compute-auth-token \
          --fleet-id fleet-1234 \
          --compute-name DevLaptop
      ```

      Example output:

      ```
      ComputeAuthToken {
        FleetId = fleet-1234,
        ComputeName = DevLaptop,
        AuthToken = hijklmnop456,
        ExpirationTime = 1897492857.11
      }
      ```

   1. Run a debug instance of your game server executable\. To run the debug instance, your game server must call `InitSDK()`\. After the process is ready to host a game session, the game server calls `ProcessReady()`\.

1. Your fleet now has an available server process\. Create your game session and perform your next tests\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html) command \(or the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html) API operation\)\.

   ```
   aws gamelift create-game-session \
       --fleet-id fleet-1234 \
       --name SecondDebugSession \
       --maximum-player-session-count 2
   ```

   GameLift sends an `onCreateGameSession()` message to your registered server process\. The message contains the `GameSession` object from the previous step with game properties, game sessions data, matchmaker data, and more about the game session\.

1. Add logic to your game server so that your server process responds to the `onCreateGameSession()` message with `ActivateGameSession()`\. The operation sends an acknowledgement to GameLift that your server received and accepted the create game session message\. For more information see, [Amazon GameLift server SDK reference](reference-serversdk.md)\.

After you finish testing your game server, you can continue to use GameLift for your fleet and game server management\. For more information, see [Create a GameLift Anywhere fleet](fleets-creating-anywhere.md)\.