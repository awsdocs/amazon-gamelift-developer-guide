# Installing and setting up the plug\-in for Unity<a name="unity-plug-in-install"></a>

This section explains downloading, installing, and setting up the Amazon GameLift Plug\-in for Unity\. 

**Prerequisites**
+ Unity for Windows 2019\.4 LTS, Windows 2020\.3 LTS, or Unity for MacOS
+ Current version of Java
+ Current version of \.NET 4\.x

**To download and install the Plug\-in for Unity**

1. Download the Amazon GameLift Plug\-in for Unity\. You can find the latest version on the [Amazon GameLift Plug\-in for Unity repository](https://github.com/aws/amazon-gamelift-plugin-unity/releases) page\. Under the [latest release](https://github.com/aws/amazon-gamelift-plugin-unity/releases), choose **Assets**, and then download the `com.amazonaws.gamelift-version.tgz` file\. 

1. Launch Unity and choose a project\.

1. In the top navigation bar, under **Window** choose **Package Manager**:  
![\[Unity menu under Window with package manager selected.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_pkgmgr.png)

1. Under the **Package Manager** tab choose **\+**, and then choose **Add package from tarball\.\.\.**:  
![\[Add package from tarball highlighted under the + icon in the Package Manager tab.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_tarball.png)

1. In the **Select packages on disk** window, navigate to the `com.amazonaws.gamelift` folder, choose the file `com.amazonaws.gamelift-version.tgz `, and then choose **Open**:  
![\[Choosing the tarball file in the select package on disk window.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_tarballselect.png)

1. After Unity has loaded the plug\-in, **GameLift** appears as a new item in the Unity menu\. It may take a few minutes to install and recompile scripts\. The **GameLift Plugin Settings** tab automatically opens\.  
![\[Amazon GameLift Plug-in for Unity plugin settings menu.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/unitypi_install_done_ui.png)

1. In the **SDK** pane, choose **Use \.NET 4\.x**\.

   When configured, the status changes from **Not Configured** to **Configured**\. 