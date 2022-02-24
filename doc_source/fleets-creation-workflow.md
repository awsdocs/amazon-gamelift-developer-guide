# How GameLift fleet creation works<a name="fleets-creation-workflow"></a>

When you create a new fleet, the GameLift service initiates a workflow that creates a fleet with one instance in each fleet location\. As each step of the workflow is completed, events are emitted and status is updated\. You can track all events, including those for fleet creation, using the GameLift console \(see the Fleet detail page, **Events** tab\) or by calling the GameLift API [DescribeFleetEvents](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html)\. You can track the status of individual locations using [DescribeFleetLocationAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html)\.

**Note**  
You cannot remotely access an instance in a fleet until the fleet is in ACTIVE status\.

Fleet creation workflow:
+ GameLift creates a Fleet resource\. For the fleet's home Region and each remote location that is defined in the fleet, the desired capacity is set to one \(1\) instance\. Fleet/location status is set to **New**, and GameLift begins writing events to the fleet event log\.
+ GameLift allocates requested computing resources for one new instance in each fleet location\.
+ GameLift downloads the game server files to each instance\. The files are either the custom game build or a Realtime Servers build with custom configuration script\. Status is set to **Downloading**\. 
+ The downloaded game server files on each instance are validated to ensure that no errors occurred during downloading\. Status is set to **Validating**\.
+ The game server is built on each instance\. For a custom game build, the game server is installed using an install script if available\. For Realtime Servers, the software is installed using the configuration script\. Status is set to **Building**\.
+ GameLift begins launching server processes on each instance, following instructions in the fleet's runtime configuration\. If the fleet is configured to run multiple concurrent server processes per instance, GameLift staggers the process launches by a few seconds\. As each process comes online, it reports readiness back to the GameLift service\. Status is set to **Activating**\.
+ As soon as one server process in any fleet location notifies GameLift that it is ready, the fleet status and the location status is set to **Active**\. As server processes in other fleet locations report readiness, the status of each fleet location is set to **Active**\.

To troubleshoot issues with fleet creation, see [Debug GameLift fleet issues](fleets-creating-debug.md)\.