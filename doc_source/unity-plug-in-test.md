# Testing your game locally<a name="unity-plug-in-test"></a>

Use Amazon GameLift Local to run GameLift on your local device\. You can use GameLift Local to verify code changes in seconds, without a network connection\.

## Configuring local testing<a name="unity-plug-in-test-cfgtesting"></a>

1. In the Plug\-in for Unity window, choose the **Test** tab\.

1. In the **Test** pane, choose **Download GameLift Local**\. The Plug\-in for Unity opens a browser window and downloads the `GameLift_06_03_2021.zip` file to your downloads folder\.

   The download includes the C\# Server SDK, \.NET source files, and a \.NET component compatible with Unity\.

1. Unzip the downloaded file `GameLift_06_03_2021.zip`\. 

1. In the **GameLift Plugin Settings** window, choose **GameLift Local Path**, navigate to the unzipped folder, choose the file `GameLiftLocal.jar`, and then choose **Open**\.

   When configured, local testing status changes from **Not Configured** to **Configured**\.

1. Verify the status of the JRE\. If the status is **Not Configured**, choose **Download JRE** and install the recommended Java version\.

   After you install and configure the Java environment, the status changes to **Configured**\.

## Running your local game<a name="unity-plug-in-test-cfgtesting"></a>

1. In the Plug\-in for Unity tab, choose the **Test** tab\.

1. In the **Test** pane, choose **Open Local Test UI**\.

1. In the **Local Testing** window, specify a **Server executable path**\. Select **\.\.\.** to select the path and executable name of your server application\.

1. In the **Local Testing** window, specify a **GL Local port**\.

1. Choose **Deploy & Run** to deploy and run the server\.

1. To stop your game server, choose **Stop** or close the game server windows\.