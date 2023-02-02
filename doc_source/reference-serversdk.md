# Amazon GameLift server SDK reference<a name="reference-serversdk"></a>

This section contains reference documentation for the Amazon GameLift Server SDK\. Use the Server SDK to integrate your custom game server with the Amazon GameLift service to start and manage game servers as needed\.

## Migrate to server SDK version 5\.x<a name="fleet-anywhere-sdk"></a>

Consider the following changes when you migrate from an earlier version of the server SDK to server SDK version 5\.x\.
+ Download and replace the GameLift server SDK with the latest version\.
+ The existing `onStartGameSession()` callback from the server SDK is now `onCreateGameSession()`\.
+ `InitSDK()` requires multiple inputs\. 
  + When using an managed EC2 fleet, the RuntimeConfiguration or EC2 metadata provides all the required inputs\. Read the metadata and pass it as part of `InitSdk()`\.
  + When using an GameLift Anywhere fleet define these values as environment variables and pass them as part of the `InitSdk()` call\.
+ Update automations created on earlier versions of the SDK to supply the `SdkVersion`\. If the automation doesn't provide an `SdkVersion`, the value defaults to 4\.\*\.
+ SDK 5\.0 emits the metrics `ActiveCompute` using CloudWatch dimensions `FleetId`, `Location`, and `ComputeType`\.

**Topics**
+ [Migrate to server SDK version 5\.x](#fleet-anywhere-sdk)
+ [GameLift server API reference for C\+\+](integration-server-sdk-cpp-ref.md)
+ [GameLift server API reference for C\#](integration-server-sdk-csharp-ref.md)
+ [GameLift server API reference for Go](integration-server-sdk-go-ref.md)
+ [GameLift server API reference for Unreal Engine](integration-server-sdk-unreal-ref.md)