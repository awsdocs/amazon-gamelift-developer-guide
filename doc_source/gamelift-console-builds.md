# View Your Builds<a name="gamelift-console-builds"></a>

You can view information about all the game server builds you have uploaded to Amazon GameLift and take actions on them\. Builds shown include only those uploaded for the selected region\.

## Build Catalog<a name="gamelift-console-builds-catalog"></a>

Uploaded builds are shown on the **Builds** page\. To view this page, choose **Builds** from the Amazon GameLift console menu bar\.

The **Builds** page provides the following summary information for all builds: 
+ **Status** – Displays one of three possible status messages:
  + **Initialized** – The build has been created, but the upload has not yet started or the upload is still in progress\.
  + **Ready** – The build has been successfully received and is ready for fleet creation\.
  + **Failed** – The build timed out before the binaries were received\.
+ **Build name** – Name associated with the uploaded build\. A build name is provided when uploading the build to Amazon GameLift, and can be changed using the AWS SDK action [UpdateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateBuild.html)\.
+ **Build ID** – Unique ID assigned to the build on upload\.
+ **Version** – Version label associated with the uploaded build\. A build name is provided when uploading the build to Amazon GameLift, and can be changed using the AWS SDK action [UpdateBuild](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateBuild.html)\.
+ **OS** – Operating system that the build runs on\. The build OS determines what operating system is installed on a fleet's instances\.
+ **Size** – Size, in megabytes \(MB\) of the build file uploaded to Amazon GameLift\.
+ **Date created** – Date and time that the build was uploaded to Amazon GameLift\.
+ **Fleets** – Number of fleets currently deployed with this build\.

From this page you can do any of the following: 
+ Create a new fleet from a build\. Select a build and click **Create fleet from build**\.
+ Delete a build\. Select a build and click **Delete build**\.
+ Filter and sort the build list\. Use the controls at the top of the table\.
+ View build details\. Click a build name to open the build detail page\.

## Build Detail<a name="gamelift-console-builds-detail"></a>

Access a build's detail page from either the console dashboard or the **Builds** page by clicking the build name\. The **Build** detail page displays the same build summary information as the Builds page\. It also shows a list of fleets created with the build\. This list is essentially the fleets catalog, filtered by build\. It includes the same summary information as the [**Fleets** page](gamelift-console-fleets.md)\.