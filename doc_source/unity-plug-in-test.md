# Testing your game locally<a name="unity-plug-in-test"></a>

You can test builds of your game server on your local machine\. Once you launch your game server, you can launch your game client and debug GameLift methods and other interactions\.

**Topics**
+ [Configuring local testing](#unity-plug-in-test-cfgtesting)
+ [Testing your game locally](#unity-plug-in-test-cfgtesting)

## Configuring local testing<a name="unity-plug-in-test-cfgtesting"></a>

Amazon GameLift Local is a client\-side debugging tool that emulates a subset of the GameLift API on your local development machine\. You can use GameLift Local to verify code changes in seconds, without a network connection\.

**To configure GameLift local**

1. In Unity, in the Plug\-in for Unity tab, select the **Test** tab\.

1. In the **Test** pane, select **Download GameLift Local**\. The Plug\-in for Unity will open a browser window and download `GameLift_06_03_2021.zip` to your downloads folder\. Your browser window may automatically close\. 

   The Server SDK zip file includes the C\# Server SDK, \.NET source files and a pre\-built \.NET component suitable for Unity\.

1. Unzip the downloaded file `GameLift_06_03_2021.zip`\. 

1. In the **GameLift Plugin Settings** window, select **GameLift Local Path** window, navigate to the unzipped folder, select the file `GameLiftLocal.jar`, and then choose **Open**\.

   When configured, local testing status will change from **Not Configured** to **Configured**\.

1. Verify the status of the JRE\. If it is **Not Configured** select **Download JRE**and install the recommended Java version\.

   Once the Java environment has been installed and configured, the status will change to **Configured**\.

1. The Plug\-in for Unity can launch your game server\. Select **Open Local Test UI** and then specify the **Game Server \.exe File Path**, and then choose **Deploy and Run**\. You can stop your game server by choosing **Stop** or by closing the game server windows\. 

## Testing your game locally<a name="unity-plug-in-test-cfgtesting"></a>

The Amazon GameLift Plug\-in for Unity makes it easy to launch your game server for local testing\. You can launch as many game clients as you need to test your game\.

**To test your game locally**

1. In Unity, in the Plug\-in for Unity tab, select the **Test** tab\.

1. In the **Test** pane, select **Open Local Test UI**\.

1. In the **Local Testing** window, specify a **Server executable path**\. Select **\.\.\.** to select the path and executable name of your server application\.

1. In the **Local Testing** window, specify a **GL Local port**\.

1. Select **Deploy & Run** to deploy and run the server\.

1. When local testing is completed, select **Stop**\.

   You can also manually close the game server windows\.