# Realtime Servers Client API \(C\#\) Reference<a name="realtime-sdk-csharp-ref"></a>

Use the Realtime Client API to prepare your multiplayer game clients for use with Amazon GameLift Realtime Servers\. For more on the integration process, see [Get Started with Realtime Servers](realtime-plan.md)\. The Client API contains a set of synchronous API calls and asynchronous callbacks that enable a game client to connect to a Realtime server and exchange messages and data with other game clients via the server\. 

This API is defined in the following libraries: 

Client\.cs 
+ [Synchronous Actions](realtime-sdk-csharp-ref-actions.md)
+ [Asynchronous Callbacks](realtime-sdk-csharp-ref-callbacks.md)
+ [Data Types](realtime-sdk-csharp-ref-datatypes.md)

**To set up the Realtime Client API**

1. **Download the [Amazon GameLift Realtime Client SDK](https://aws.amazon.com/gamelift/getting-started)\.** 

1. **Build the C\# SDK libraries\.** Locate the solution file `GameScaleLightweightClientSdkNet45.sln`\. See the `README.md` file for the C\# Server SDK for minimum requirements and additional build options\. In an IDE, load the solution file\. To generate the SDK libraries, restore the NuGet packages and build the solution\.

1. **Add the Realtime Client libraries to your game client project\.** 