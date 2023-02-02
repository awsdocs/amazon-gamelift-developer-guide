# Create a new GameLift fleet<a name="fleets-creating-all"></a>

Create a new fleet and deploy your custom game server build or Realtime Servers for hosting\. You can deploy any game build or script resource that you upload to Amazon GameLift\.

**Topics**
+ [How GameLift fleet creation works](#fleets-creation-workflow)
+ [Create a managed fleet](fleets-creating.md)
+ [Create a GameLift Anywhere fleet](fleets-creating-anywhere.md)

## How GameLift fleet creation works<a name="fleets-creation-workflow"></a>

When you create a new fleet, GameLift starts a workflow that creates a fleet with one Amazon Elastic Compute Cloud \(Amazon EC2\) instance in each fleet location\. As GameLift completes each step of the workflow, the fleet emits events and GameLift updates the fleet's status\. You can track all events using the GameLift console or by calling the GameLift API operation [DescribeFleetEvents](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetEvents.html)\. You can also track the status of individual locations using [DescribeFleetLocationAttributes](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeFleetLocationAttributes.html)\.

**EC2 fleet creation workflow:**
+ GameLift creates a fleet resource in the fleet's home Region and in each remote location defined in the fleet\.
+ GameLift sets the desired capacity to one instance\.
+ GameLift sets the fleet and location status to **New**\.
+ GameLift begins writing events to the fleet event log\.
+ GameLift allocates requested computing resources for one new instance in each fleet location\.
+ GameLift downloads the game server files to each instance and sets the fleet status to **Downloading**\.
+ GameLift validates the downloaded game server files on each instance to verify that no errors occurred during downloading\. GameLift sets the fleet status to **Validating**\.
+ GameLift builds the game server on each instance and sets the fleet status to **Building**\.
+ GameLift begins launching server processes on each instance, following instructions in the fleet's runtime configuration\. If you configured the fleet to run multiple concurrent server processes per instance, then GameLift staggers the process launches by a few seconds\. As each process comes online, it reports readiness back to GameLift\. GameLift sets the fleet status to **Activating**\.
+ GameLift sets the fleet statuses and the location statuses to **Active** as server processes report readiness\.

**GameLift Anywhere fleet creation**
+ GameLift creates a fleet resource\. For the fleet's home Region and each custom location defined in the fleet, GameLift sets the fleet and location status to **New**\.
+ GameLift begins writing events to the fleet event log\.
+ After one server process in a fleet notifies GameLift that it's ready, GameLift sets the fleet status and the location status to **Active**\. As server processes in other fleet locations report readiness, GameLift sets the status of each fleet location to **Active**\.

For help with troubleshooting fleet creation issues, see [Debug GameLift fleet issues](fleets-creating-debug.md)\.