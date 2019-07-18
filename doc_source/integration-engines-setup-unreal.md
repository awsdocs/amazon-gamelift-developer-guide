# Add Amazon GameLift to an Unreal Engine Game Server Project<a name="integration-engines-setup-unreal"></a>

This topic helps you set up and use the Amazon GameLift Server SDK plugin for Unreal Engine in your game server projects\. If you're unsure whether Amazon GameLift supports the operating systems you're using, see [For Custom Game Servers](gamelift-supported.md#gamelift-supported-servers)\.

## Set Up the Unreal Engine Server SDK Plugin<a name="integration-engines-setup-unreal-setup"></a>

Follow these steps to get the Amazon GameLift Server SDK plugin for Unreal Engine ready for your game server projects\.

**To set up the Amazon GameLift SDK plugin for Unreal Engine**

1. **Download the [Amazon GameLift Server SDK](https://aws.amazon.com/gamelift/getting-started)\.** To verify that your game system requirements are supported, see [Amazon GameLift SDKs](gamelift-supported.md)\.

1. **Build the C\+\+ Server SDK libraries for Unreal\.** The SDK download contains the source code for C\+\+ \(see `GameLift_<release date>\GameLift-SDK-Release-<version>\GameLift-cpp-ServerSDK-<version>`\)\. Check the README file in this directory for minimum requirements and additional information before building the SDK\.

   To build the SDK libraries, go to the directory `GameLift-cpp-ServerSDK-<version>` and compile with the flag `-DBUILD_FOR_UNREAL` set to true\. The following instructions show how to compile using cmake\.

   For Linux users:

   ```
   mkdir out
   cd out
   cmake -DBUILD_FOR_UNREAL=1 ..
   make
   ```

   The following binary files are generated:
   + `out/prefix/lib/libaws-cpp-sdk-gamelift-server.so`

   For Windows users:

   ```
   mkdir out
   cd out
   cmake -G "Visual Studio 15 2017 Win64" -DBUILD_FOR_UNREAL=1 ..
   msbuild ALL_BUILD.vcxproj /p:Configuration=Release
   ```

   The following binary files are generated:
   + `out\prefix\bin\aws-cpp-sdk-gamelift-server.dll`
   + `out\prefix\lib\aws-cpp-sdk-gamelift-server.lib`

   For more details on building the C\+\+ SDK, including minimum requirements and build options, see the `README.md` file included in the download\. 

1. **Add the binaries to the Amazon GameLift plugin files\.** Open the directory for the plugin version of UE4 that you are working with \(for example, `GameLift-SDK-Release-3.3.0\GameLift-Unreal-plugin-3.3.0\UE4.21.1\GameLiftServerSDK`\)\. Copy the binary files that you created in Step 2 into the `ThirdParty` directory of the Unreal plugin:

   For Linux use these paths:
   + `.../ThirdParty/GameLiftServerSDK/Linux/x86_64-unknown-linux-gnu/aws-cpp-sdk-gamelift-server.so`

   for Windows use these paths:
   + `...\ThirdParty\GameLiftServerSDK\Win64\aws-cpp-sdk-gamelift-server.dll`
   + `...\ThirdParty\GameLiftServerSDK\Win64\aws-cpp-sdk-gamelift-server.lib`

1. **Import the Amazon GameLift plugin into a project\.** There are many ways to import a plugin into Unreal Engine\. The following method does not require the Unreal Editor\.

   1. Add the plugin to your game project\. The plugin files must contain everything in the plugin's `GameLiftServerSDK` directory, including the generated binary files\.

   1. Add the plugin to your game's `.uproject` file:

      ```
      "Plugins": [
      	{
      		"Name": "GameLiftServerSDK",
      		"Enabled": true
      	}
      ]
      ```

   1. Add the plugin name as a dependency to your game's list of ModuleRules\. The following example shows a sample list of module names with the Amazon GameLift plugin added to it\.

      ```
      using UnrealBuildTool;
      
      public class MyAwesomeGame : ModuleRules
      {
          public MyAwesomeGame(TargetInfo Target)
          {
              PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "GameLiftServerSDK" });
          }
      }
      ```

## Add Amazon GameLift Code<a name="integration-engines-setup-unreal-code"></a>

For more information on adding Amazon GameLift functionality, see these topics: 
+ [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)
+ [Amazon GameLift Server API Reference for Unreal Engine](integration-server-sdk-unreal-ref.md)

When adding Amazon GameLift\-specific code to your Unreal Engine game project, enclose the code using the preprocessor flag `WITH_GAMELIFT=1`\. This flag ensures that only server builds invoke the Amazon GameLift backplane API and allows you to write code that is executed correctly regardless of the build target type you might produce with it\.

Code enclosed with the `WITH_GAMELIFT=1` flag is only processed if the following are true: 
+ The plugin found the Amazon GameLiftserver SDK binary files\.
+ The build is a game server: `Target.Type == TargetRules.TargetType.Server`

The following code snippet illustrates how to initialize an Unreal Engine game server with Amazon GameLift\. 

```
//This is an example of a simple integration with GameLift server SDK that makes game server 
//processes go active on Amazon GameLift

// Include game project files. "GameLiftFPS" is a sample game name, replace with file names from your own game project
#include "GameLiftFPSGameMode.h"
#include "GameLiftFPS.h"
#include "Engine.h"
#include "EngineGlobals.h"
#include "GameLiftFPSHUD.h"
#include "GameLiftFPSCharacter.h"
#include "GameLiftServerSDK.h"

AGameLiftFPSGameMode::AGameLiftFPSGameMode()
    : Super()
{

    //Let's run this code only if GAMELIFT is enabled. Only with Server targets!
#if WITH_GAMELIFT

    //Getting the module first.
    FGameLiftServerSDKModule* gameLiftSdkModule = &FModuleManager::LoadModuleChecked<FGameLiftServerSDKModule>(FName("GameLiftServerSDK"));

    //InitSDK establishes a local connection with GameLift's agent to enable communication.
    gameLiftSdkModule->InitSDK();

    //Respond to new game session activation request. GameLift sends activation request 
    //to the game server along with a game session object containing game properties 
    //and other settings. Once the game server is ready to receive player connections, 
    //invoke GameLiftServerAPI.ActivateGameSession()
    auto onGameSession = [=](Aws::GameLift::Server::Model::GameSession gameSession)
    {
        gameLiftSdkModule->ActivateGameSession();
    };
    
    FProcessParameters* params = new FProcessParameters();
    params->OnStartGameSession.BindLambda(onGameSession);

    //OnProcessTerminate callback. GameLift invokes this before shutting down the instance 
    //that is hosting this game server to give it time to gracefully shut down on its own. 
    //In this example, we simply tell GameLift we are indeed going to shut down.
    params->OnTerminate.BindLambda([=](){gameLiftSdkModule->ProcessEnding();});

    //HealthCheck callback. GameLift invokes this callback about every 60 seconds. By default, 
    //GameLift API automatically responds 'true'. A game can optionally perform checks on 
    //dependencies and such and report status based on this info. If no response is received  
    //within 60 seconds, health status is recorded as 'false'. 
    //In this example, we're always healthy!
    params->OnHealthCheck.BindLambda([](){return true; });

    //Here, the game server tells GameLift what port it is listening on for incoming player 
    //connections. In this example, the port is hardcoded for simplicity. Since active game
    //that are on the same instance must have unique ports, you may want to assign port values
    //from a range, such as:
    //const int32 port = FURL::UrlConfig.DefaultPort;
    //params->port;
    params->port = 7777;

    //Here, the game server tells GameLift what set of files to upload when the game session 
    //ends. GameLift uploads everything specified here for the developers to fetch later.
    TArray<FString> logfiles;
    logfiles.Add(TEXT("aLogFile.txt"));
    params->logParameters = logfiles;

    //Call ProcessReady to tell GameLift this game server is ready to receive game sessions!
    gameLiftSdkModule->ProcessReady(*params);
#endif
}
```