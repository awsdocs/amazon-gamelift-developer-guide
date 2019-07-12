# Add Amazon GameLift to a Unity Game Server Project<a name="integration-engines-unity-using"></a>

This topic helps you set up the Amazon GameLift C\# Server SDK and integrate Amazon GameLift into your Unity game server projects\. If you're unsure whether Amazon GameLift supports the operating systems you're using, see [For Custom Game Servers](gamelift-supported.md#gamelift-supported-servers)\.

## Set up the C\# Server SDK for Unity<a name="integration-engines-unity-setup"></a>

Follow these steps to build the Amazon GameLift Server SDK for C\# and add it to your Unity game server projects\.

**To set up the Amazon GameLift Server SDK for Unity**

1. **Download the [Amazon GameLift Server SDK](https://aws.amazon.com/gamelift/getting-started)\.** To verify that your game system requirements are supported, see [Amazon GameLift SDKs](gamelift-supported.md)\. The Server SDK includes the following two solutions, both of which can be used with Unity:
   + `GameLiftServerSDKNet35.sln` for \.Net framework 3\.5 
   + `GameLiftServerSDKNet45.sln` for \.Net framework 4\.5

1. **Build the C\# SDK libraries\.** See the `README.md` file for the C\# Server SDK for minimum requirements and additional build options\. In an IDE, load the solution file that you want to use\. To generate the SDK libraries, restore the NuGet packages and build the solution\.

1. **Check the Configuration settings\.** In the Unity Editor, go to **File**, **Build Settings**, **Player Settings**\. Under **Other Settings**, **Configuration**, check the following settings: 
   + Scripting Runtime Version: Set to the \.NET solution you're using\.

1. **Add the Amazon GameLift libraries to Unity\.** In the Unity Editor, import the libraries that were produced by the build into the `Assets/Plugins` directory of your project\.

## Add Amazon GameLift Server Code<a name="integration-engines-unity-code"></a>

For more information on adding Amazon GameLift functionality, see these topics: 
+ [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)
+ [Amazon GameLift Server API \(C\#\) Reference](integration-server-sdk-csharp-ref.md)

The following code example uses a `MonoBehavior` to illustrate a simple game server initialization with Amazon GameLift\.

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
        //Make sure to call GameLiftServerAPI.Destroy() when the application quits. 
        //This resets the local connection with GameLift's agent.
        GameLiftServerAPI.Destroy();
    }
}
```