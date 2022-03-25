# Installing the plug\-in for Unity<a name="unity-plug-in-install"></a>

In this section, you download and install the Amazon GameLift Plug\-in for Unity\. The Plug\-in for Unity requires Unity for Windows version 2019\.4 LTS and 2020\.3 LTS\. To build and test games using GameLift Local, the GameLift Managed Servers SDK is required\. You will also need to install the current version of Java and \.NET 4\.x\.

**To download and install the Plug\-in for Unity**

1. Download the Amazon GameLift Plug\-in for Unity\. You can find the latest version on the [Amazon GameLift Plug\-in for Unity repository](https://github.com/aws/amazon-gamelift-plugin-unity/releases) page\. Under the [latest release](https://github.com/aws/amazon-gamelift-plugin-unity/releases), select **Assets**, and then download ` com.amazonaws.gamelift-version.tgz`\. 

1. Launch Unity and select a project\.

1. On the menu, select **Window**, and then choose **Package Manager**:  
![\[Unity menu with package manager selected\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_pkgmgr.png)

1. In the **Package Manager** tab, under the tab, select **\+**, and then choose **Add package from tarball\.\.\.**:  
![\[Install from tarball\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_tarball.png)

1. In the **Select packages on disk** window, navigate to the `com.amazonaws.gamelift` folder, select the file `com.amazonaws.gamelift-version.tgz `, and then choose **Open**:  
![\[Selecting tarball\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_tarballselect.png)

1. Once the Plug\-in for Unity is loaded, **GameLift** will be added as a new item on the Unity menu\. It may take a few minutes to install and recompile scripts\. The **GameLift Plugin Settings** tab will automatically open:  
![\[Amazon GameLift Plug-in for Unity installed\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_done_ui.png)

1. Close the window\. To configure \.NET settings, proceed to [Configuring \.NET settings](unity-plug-in-configure-net.md)\. 