# Add Amazon GameLift to a unity game server project<a name="integration-engines-unity-using"></a>

This topic helps you set up the GameLift C\# Server SDK in your Unity game server projects\. If you're unsure whether the managed GameLift service supports the operating systems you're using, see [For custom game servers](gamelift-supported.md#gamelift-supported-servers)\.

## Set up the C\# server SDK for unity<a name="integration-engines-unity-setup"></a>

Follow these steps to build the GameLift Server SDK for C\# and add it to your Unity game server projects\.

**To set up the GameLift server SDK for unity**

1. **Download the [GameLift server SDK](https://aws.amazon.com/gamelift/getting-started)\.** To verify that your game system requirements are supported, see [GameLift SDKs](gamelift-supported.md)\. The Server SDK zip file includes the C\# Server SDK, with source files so that you can build the SDK as needed for your project\. 

1. **Build the C\# SDK libraries\.** In an IDE, load the C\# Server SDK solution file that you want to use\. Use the IDE's functionality to restore NuGet files for the project\. See the `README.md` file for the C\# Server SDK for minimum requirements and additional build options\. Build the solution to generate the C\# SDK libraries\.

1. **Check the Configuration settings\.** In the Unity Editor, open your game project\. Go to **File**, **Build Settings**, **Player Settings**\. Under **Other Settings**, **Configuration**, check the following settings: 
   + Scripting Runtime Version: Set to the \.NET solution you're using\.

1. **Add the GameLift libraries to your Unity project\.** In the Unity Editor, import the libraries that were produced by the solution build into the `Assets/Plugins` directory of your project\. See the `README.md` file for a complete list of the libraries for the SDK version that you're using\.

## Add GameLift server code<a name="integration-engines-unity-code"></a>

For more information on adding GameLift functionality, see these topics: 
+ [Add GameLift to your game server](gamelift-sdk-server-api.md)
+ [GameLift server API reference for C\#](integration-server-sdk-csharp-ref.md)

The following code example uses a `MonoBehavior` to illustrate a simple game server initialization with GameLift\.

```
using UnityEngine;
using Aws.GameLift.Server;
using System.Collections.Generic;

public class GameLiftServerExampleBehavior : MonoBehaviour
{
    //This is an example of a simple integration with GameLift server SDK that makes game server 
    //processes go active on Amazon GameLift
    public void Start()
    {
        //Set the port that your game service is listening on for incoming player connections (hard-coded here for simplicity)
        var listeningPort = 7777;

        //InitSDK establishes a local connection with the Amazon GameLift agent to enable 
        //further communication.
        var initSDKOutcome = GameLiftServerAPI.InitSDK();
        if (initSDKOutcome.Success)
        {
            ProcessParameters processParameters = new ProcessParameters(
                (gameSession) => {
                    //Respond to new game session activation request. GameLift sends activation request 
                    //to the game server along with a game session object containing game properties 
                    //and other settings. Once the game server is ready to receive player connections, 
                    //invoke GameLiftServerAPI.ActivateGameSession()
                    GameLiftServerAPI.ActivateGameSession();
                },
                () => {
                    //OnProcessTerminate callback. GameLift invokes this callback before shutting down 
                    //an instance hosting this game server. It gives this game server a chance to save
                    //its state, communicate with services, etc., before being shut down. 
                    //In this case, we simply tell GameLift we are indeed going to shut down.
                    GameLiftServerAPI.ProcessEnding();
                }, 
                () => {
                    //This is the HealthCheck callback.
                    //GameLift invokes this callback every 60 seconds or so.
                    //Here, a game server might want to check the health of dependencies and such.
                    //Simply return true if healthy, false otherwise.
                    //The game server has 60 seconds to respond with its health status. 
                    //GameLift will default to 'false' if the game server doesn't respond in time.
                    //In this case, we're always healthy!
                    return true;
                },
                //Here, the game server tells GameLift what port it is listening on for incoming player 
                //connections. In this example, the port is hardcoded for simplicity. Active game
                //that are on the same instance must have unique ports.
                listeningPort, 
                new LogParameters(new List<string>()
                {
                    //Here, the game server tells GameLift what set of files to upload when the game session ends.
                    //GameLift uploads everything specified here for the developers to fetch later.
                    "/local/game/logs/myserver.log"
                }));

            //Calling ProcessReady tells GameLift this game server is ready to receive incoming game sessions!
            var processReadyOutcome = GameLiftServerAPI.ProcessReady(processParameters);
            if (processReadyOutcome.Success)
            {
                print("ProcessReady success.");
            }
            else
            {
                print("ProcessReady failure : " + processReadyOutcome.Error.ToString());
            }
        }
        else
        {
            print("InitSDK failure : " + initSDKOutcome.Error.ToString());
        }
    }

    void OnApplicationQuit()
    {
        //Make sure to call GameLiftServerAPI.ProcessEnding() when the application quits. 
        //This resets the local connection with GameLift's agent.
        GameLiftServerAPI.ProcessEnding();
    }
}
```