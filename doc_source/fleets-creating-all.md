# Create a new GameLift fleet<a name="fleets-creating-all"></a>

Create a new fleet and deploy your custom game server build or Realtime Servers for hosting\. You can deploy any game build or script resource that has been successfully uploaded to the Amazon GameLift service\. 

**Tip**  
To learn more about GameLift features, including Realtime Servers, [try out the GameLift sample games](gamelift-explore.md)\.

**Topics**
+ [How GameLift fleet creation works](#fleets-creation-workflow)
+ [Deploy a fleet](fleets-creating.md)

## How GameLift fleet creation works<a name="fleets-creation-workflow"></a>

When you create a new fleet, GameLift starts a workflow that creates a fleet with one instance in each fleet location\. As GameLift completes each step of the workflow, the fleets emit events and GameLift updates the fleet status\. You can track all events using the GameLift console or by calling the GameLift API [DescribeFleetEvents](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html)\. You can track the status of individual locations using [DescribeFleetLocationAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html)\.

**Fleet creation workflow:**
+ GameLift creates a Fleet resource\. For the fleet's home Region and each remote location defined in the fleet, GameLift sets the desired capacity to one instance\. GameLift sets the fleet and location status to **New**, and GameLift begins writing events to the fleet event log\.
+ GameLift allocates requested computing resources for one new instance in each fleet location\.
+ GameLift downloads the game server files to each instance, and sets the fleet status to **Downloading**\. 
+ GameLift validates the downloaded game server files on each instance to verify that no errors occurred during downloading\. GameLift sets the fleet status to **Validating**\.
+ GameLift builds the game server on each instance and sets the fleet status to **Building**\.
+ GameLift begins launching server processes on each instance, following instructions in the fleet's runtime configuration\. If you configured the fleet to run multiple concurrent server processes per instance, GameLift staggers the process launches by a few seconds\. As each process comes online, it reports readiness back to the GameLift service\. GameLift sets the fleet status to **Activating**\.
+ As soon as one server process in any fleet location notifies GameLift that it's ready, GameLift sets the fleet status and the location status to **Active**\. As server processes in other fleet locations report readiness, GameLift sets the status of each fleet location to **Active**\.

To troubleshoot issues with fleet creation, see [Debug GameLift fleet issues](fleets-creating-debug.md)\.