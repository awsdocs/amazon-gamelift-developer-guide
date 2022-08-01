# View your builds<a name="gamelift-console-builds"></a>

On the **Builds** page of the [GameLift console](https://console.aws.amazon.com/gamelift/), you can view information about and manage all the game server builds that you've uploaded to GameLift\. In the navigation pane, choose **Hosting**, **Builds**\.

The **Builds** page shows the following information for each build:

**Note**  
The **Builds** page shows builds in your current AWS Region only\.
+ **Name** – The name associated with the uploaded build\.
+ **Status** – The status of the build\. Displays one of three status messages:
  + **Initialized** – The upload hasn't started or is still in progress\.
  + **Ready** – The build is ready for fleet creation\.
  + **Failed** – The build timed out before GameLift received the binaries\.
+ **Creation time** – The date and time that you uploaded the build to GameLift\.
+ **Build ID** – The unique ID assigned to the build on upload\.
+ **Version** – The version label associated with the uploaded build\.
+ **Operating system** – The OS that the build runs on\. The build OS determines which operating system GameLift installs on a fleet's instances\.
+ **Size** – The size, in megabytes \(MB\), of the build file uploaded to GameLift\.
+ **Fleets** – The number of fleets deployed with the build\.

From this page you can do any of the following:
+ View build details\. Choose a build's name to open its build details page\.
+ Create a new fleet from a build\. Select a build, and then choose **Create fleet**\.
+ Filter and sort the build list\. Use the controls at the top of the table\.
+ Delete a build\. Select a build, and then choose **Delete**\.

## Build details<a name="gamelift-console-builds-detail"></a>

On the **Builds** page, choose a build's name to open its details page\. The **Overview** section of the details page displays the same build summary information as the **Builds** page\. The **Fleets** section shows a list of fleets created with the build, including the same summary information as the [**Fleets** page](gamelift-console-fleets.md)\.