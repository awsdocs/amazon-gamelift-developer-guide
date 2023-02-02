# Create a GameLift Anywhere fleet<a name="fleets-creating-anywhere"></a>

Use Amazon GameLift to integrate hardware from your environment into your GameLift game hosting\. GameLift Anywhere registers your hardware with GameLift in an Anywhere fleet\. You can integrate Anywhere and managed EC2 fleets in matchmaker and game session queues to manage matchmaking and game placement\.

To get started, download the GameLift server SDK version 5 or greater and review the following important concepts for using a GameLift Anywhere fleet\.

**Custom locations**  
GameLift Anywhere fleets use custom locations to represent the physical locations of your infrastructure\.

**Device registration**  
For a GameLift Anywhere fleet to communicate with your compute resources, you must first register your device\. You can complete device registration from the GameLift SDK using the [https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RegisterCompute.html) operation\. This operation uses the IP address of the device to associate it with a fleet location and communicate with GameLift\.

**Authentication tokens**  
When you initialize a game server on your compute, the GameLift Server SDK uses an auth token to authenticate your game server to GameLift\. You can re\-use the same auth token for all game servers on the same compute, up to the auth token expiration time\. To retrieve the auth token, call the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/get-compute-auth-token.html) AWS Command Line Interface \(AWS CLI\) command\. Pass the token to each game server as needed\.

**Game sessions**  
When you use a GameLift Anywhere fleet, the fleet automatically launches a game session for each server process on your compute\. Each game session on a compute uses the same authentication token created while registering the compute to a fleet location\.  
The following diagram shows a game session queue that uses FlexMatch matchmaking and multiple fleets\. The fleets include an EC2 fleet with C5 instances, an Anywhere fleet with a development laptop, and an Anywhere fleet with a customer\-hosted server rack\.

![\[A diagram of a game session queue that uses an managed EC2 fleet and two Anywhere fleets.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_hosting_flow_anywhere.png)

**Topics**
+ [Create a fleet](#fleet-anywhere-create)
+ [Migrate to managed EC2](#fleet-anywhere-migration)
+ [Host games in multiple on\-premises locations](#fleet-anywhere-locations)

## Create a fleet<a name="fleet-anywhere-create"></a>

Use either the [GameLift console](https://console.aws.amazon.com/gamelift/) or the AWS CLI to create an Anywhere fleet\.

After you create a new Anywhere fleet, the fleet's status moves from `NEW` to `ACTIVE`\. When it reaches `ACTIVE` status, the fleet is ready to host game sessions\. For help with fleet creation issues, see [Debug GameLift fleet issues](fleets-creating-debug.md)\.

------
#### [ Console ]

**To create an Anywhere fleet**

1. Open the [GameLift console](https://console.aws.amazon.com/gamelift/)\.

1. In the navigation pane, under **Hosting**, choose **Locations**\.

1. On the **Locations** page, choose **Create location**\.

1. In the **Create location** dialog box, do the following:

   1. Enter a **Location name**\. This labels the location of your hardware that GameLift uses to run your games in Anywhere fleets\. GameLift appends the name of your custom location with **custom\-**\.

   1. \(Optional\) Add tags as key\-value pairs to your custom location\. Choose **Add new tag** for each tag that you want to add\.

   1. Choose **Create**\.

1. Create an Anywhere fleet\.

   1. In the navigation pane, under **Hosting**, choose **Fleets**\.

   1. On the **Fleets** page, choose **Create fleet**\.

   1. On the **Compute type** step, choose **Anywhere**, and then choose **Next**\.

   1. On the **Fleet details** step, define the details, and then choose **Next**\.

   1. On the **Custom locations** step, select the custom location that you created, and then choose **Next**\. GameLift automatically selects the home AWS Region as the Region that you're creating the fleet in\. You can use the home Region to access and use your resources\.

   1. Complete the remaining fleet creation steps, and then choose **Submit** to create your Anywhere fleet\.

1. Register your laptop as a compute resource in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html) AWS CLI command\. Include the `fleet-id` of the fleet that you created in the previous step\. Add a `compute-name` and your laptop's `ip-address`\.

   ```
   aws gamelift register-compute \
       --compute-name HardwareAnywhere \
       --fleet-id fleet-1234 \
       --ip-address 10.1.2.3 \
       --location custom-location-1
   ```

   ```
   Compute {
       FleetId = fleet-1234,
       ComputeName = HardwareAnywhere,
       Status = ACTIVE,
       IpAddress = 10.1.2.3,
       GameLiftServiceSdkEndpoint = wss://12345678.execute-api.amazonaws.com/,
       Location = custom-location-1
   }
   ```

------
#### [ AWS CLI ]

**To create an Anywhere fleet**

1. Create a custom location using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-location.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-location.html) command\. The `location-name` labels the location of your hardware that GameLift uses to run your games in Anywhere fleets\.

   ```
   aws gamelift create-location \
       --location-name custom-location-1
   ```

   Output

   ```
   {
       Location {
           LocationName = custom-location-1
       }
   }
   ```

1. Create an Anywhere fleet using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-fleet.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-fleet.html) command\. Include your custom location in `locations`\. GameLift creates the fleet in your home Region and in the custom locations that you provide\.

   ```
   aws gamelift create-fleet \
       --name HardwareAnywhere \
       --compute-type ANYWHERE
       --locations custom-location-1
   ```

   Example output

   ```
   Fleet {
       Name = HardwareAnywhere,
       ComputeType = ANYWHERE,
       FleetId = fleet-1234,
       Status = ACTIVE,
       ...
   }
   ```

1. Register your laptop as a compute resource in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html) command\. Include the `fleet-id` returned in the previous step, and add a `compute-name` and your laptop's `ip-address`\.

   ```
   aws gamelift register-compute \
       --compute-name HardwareAnywhere \
       --fleet-id fleet-1234 \
       --ip-address 10.1.2.3 \
       --location custom-location-1
   ```

   Example output

   ```
   Compute {
       FleetId = fleet-1234,
       ComputeName = HardwareAnywhere,
       Status = ACTIVE,
       IpAddress = 10.1.2.3,
       GameLiftServiceSdkEndpoint = wss://12345678.execute-api.amazonaws.com/
       Location = custom-location-1
   }
   ```

------

## Migrate to managed EC2<a name="fleet-anywhere-migration"></a>

After you've developed your game server and you're ready to prepare for production, you can have GameLift manage your hardware\. To migrate to a managed EC2 fleet, upload your build to GameLift and create a managed EC2 fleet\. For more information about uploading your build and setting up a fleet, see [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md) and [Create a managed fleet](fleets-creating.md)\.

## Host games in multiple on\-premises locations<a name="fleet-anywhere-locations"></a>

You can integrate your on\-premises hardware with the rest of your GameLift managed fleets to manage game sessions in one place\. In the following example, there are three servers across eastern, central, and western Europe\.

------
#### [ Console ]

**Add your resources to an Anywhere fleet**

1. Open the [GameLift console](https://console.aws.amazon.com/gamelift/)\.

1. In the navigation pane, under **Hosting**, choose **Locations**\.

1. On the **Locations** page, choose **Create location**\.

1. In the **Create location** dialog box, do the following:

   1. Enter a **Location name**\. For example, **eu\-east**, **eu\-central**, or **eu\-west**\. This name labels the location of your hardware that GameLift uses to run your games in Anywhere fleets\. GameLift appends the name of your custom location with **custom\-**\.

   1. \(Optional\) Add tags as key\-value pairs to your custom location\. Choose **Add new tag** for each tag that you want to add\.

   1. Choose **Create**\.

1. Repeat the previous step until you've added the location of each server that you want to register\. In this example, *custom\-eu\-east*, *custom\-eu\-central*, and *custom\-eu\-west*\.

1. Create an Anywhere fleet\.

   1. In the navigation pane, under **Hosting**, choose **Fleets**\.

   1. On the **Fleets** page, choose **Create fleet**\.

   1. On the **Compute type** step, choose **Anywhere**, and then choose **Next**\.

   1. On the **Fleet details** step, define the details, and then choose **Next**\.

   1. On the **Custom locations** step, select the custom locations that you created, and then choose **Next**\. GameLift automatically selects the home Region as the Region that you're creating the fleet in\. You can use the home Region to access and use your resources\.

   1. Complete the remaining fleet creation steps, and then choose **Submit** to create your Anywhere fleet\.

1. Register one of your servers as a compute resource in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html) AWS CLI command\. Include the `fleet-id` of the fleet that you created in the previous step\. Add a `compute-name` and your server rack's `ip-address`\.

   ```
   aws gamelift register-compute \
       --compute-name eu-east \
       --fleet-id fleet-1234 \
       --ip-address 10.1.2.3 \
       --location custom-eu-east
   ```

   Example output

   ```
   Compute {
       FleetId = fleet-1234,
       ComputeName = eu-east,
       Status = ACTIVE,
       IpAddress = 10.1.2.3,
       GameLiftServiceSdkEndpoint = wss://12345678.execute-api.amazonaws.com/
       Location = custom-eu-east
   }
   ```

1. Repeat the previous step for each server that you want to register\.

------
#### [ AWS CLI ]

**Add your resources to an Anywhere fleet**

1. Create a custom location resource for each server using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-location.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-location.html) command\. The `location-name` labels the location of each server that GameLift uses to run your games in Anywhere fleets\.

   ```
   aws gamelift create-location --location-name custom-eu-east
   aws gamelift create-location --location-name custom-eu-central
   aws gamelift create-location --location-name custom-eu-west
   ```

   Output:

   ```
   Location {
     LocationName = custom-eu-east
   }
   Location {
     LocationName = custom-eu-central
   }
   Location {
     LocationName = custom-eu-west
   }
   ```

1. Create an Anywhere fleet using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-fleet.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-fleet.html) command\. Include the three custom `locations` that you created in the previous step\.

   ```
   aws gamelift create-fleet \
       --fleet-name EuropeFleet \
       --compute-type ANYWHERE \
       --locations Location=custom-eu-east Location=custom-eu-central Location=custom-eu-west
   ```

   Example output:

   ```
   Fleet {
     Name = EuropeFleet,
     FleetId = fleet-1234,
     ComputeType = Anywhere,
     Status = ACTIVE,
     Locations = [
       custom-eu-east, custom-eu-central, custom-eu-west
     ],
     ...
   }
   ```

1. Register one of your servers as a compute resource in the fleet that you created\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/register-compute.html) command\. Include the `fleet-id` returned in the previous step\. Add a `compute-name` and your server rack's `ip-address`\.

   ```
   aws gamelift register-compute \
       --fleet-id fleet-1234 \
       --compute-name eu-east-7 \
       --ip-address 10.1.2.3 \
       --location custom-eu-east
   ```

   Example output:

   ```
   Compute {
     FleetId = fleet-1234,
     ComputeName = eu-east-7,
     Location = eu-east,
     Status = ACTIVE,
     IpAddress = 10.1.2.3,
     GameLiftServiceSdkEndpoint = wss://12345678.execute-api.amazonaws.com/
   }
   ```

1. Repeat the previous step for each server that you want to register\.

1. Get your servers ready to handle a game session\.

   1. Get the authentication token for your laptop from the fleet that you created\.

      ```
      aws gamelift get-compute-auth-token \
          --fleet-id fleet-1234 \
          --compute-name eu-east-7
      ```

      Example output:

      ```
      ComputeAuthToken {
        FleetId = fleet-1234,
        ComputeName = eu-east-7,
        AuthToken = abcdefg123,
        ExpirationTime = 1897492857.11
      }
      ```

   1. Run an instance of your game server executable\. To run this, your game server must call `InitSDK()`\. After the process is ready to host a game session, the game server calls `ProcessReady()`\.

      Server SDK input:

      ```
      InitSdk(FleetId=fleet-1234, ComputeName=eu-east-7, ProcessId=1234-5679, ComputeAuthToken=abcdefg123, GameLiftServiceSdkEndpoint=wss://12345678.execute-api.amazonaws.com)
      ```

      ```
      ProcessReady(Port=1024)
      ```

      Output:

      ```
      Response - 200 for Success
      ```

1. Start your game session using the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/start-matchmaking.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/start-matchmaking.html), [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/start-game-session-placement.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/start-game-session-placement.html), or [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/gamelift/create-game-session.html) command\.

   ```
   aws gamelift create-game-session \
       --fleet-id fleet-1234 \
       --name EastGameSession \
       --maximum-player-session-count 2 \
       --location custom-eu-east
   ```

   Example output:

   ```
   GameSession {
     FleetId = fleet-1234,
     GameSessionId = 4444-4444,
     Name = EastGameSession,
     Location = custom-eu-east,
     IpAddress = 10.1.2.3,
     Port = 1024,
     ...
   }
   ```

   GameLift sends an `onCreateGameSession()` message to your registered server process\. The message contains the `GameSession` object from the previous step with game properties, game sessions data, matchmaker data, and more about the game session\.

1. Add logic to your game server so that your server process responds to the `onCreateGameSession()` message with `ActivateGameSession()`\. This operation has no parameters, but it sends an acknowledgement to GameLift that your server received and accepted the create game session message\.

1. When the game session is complete, end the game process\.

   Server SDK input:

   ```
   ProcessEnding()
   ```

   Output:

   ```
   Response - 200 for Success
   ```

   Then, prepare the game for the next session\.

   Server SDK input:**

   ```
   ProcessReady(Port=1024)
   ```

   Output:

   ```
   Response - 200 for Success
   ```

------