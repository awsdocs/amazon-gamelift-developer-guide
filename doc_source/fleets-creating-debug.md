# Debug Fleet Issues<a name="fleets-creating-debug"></a>

This topic provides guidance on the fleet\-related issues\. For additional troubleshooting, you can remotely access a fleet instance\. See [Remotely Access Fleet Instances](fleets-remote-access.md)\.

## Fleet Creation Issues<a name="fleets-creating-debug-creation"></a>

### How Fleet Creation Works<a name="fleets-creating-debug-howitworks"></a>

When you create a new fleet, the Amazon GameLift service performs a series of tasks to prepare a single instance based on the fleet's configuration\. As each phase of fleet creation is completed, a series of events are emitted for the fleet along with the fleet's current status\. You can track all events, including those for fleet creation, using the Amazon GameLift console \(see the Fleet detail page, Events tab\. 

Problems during fleet creation will cause the fleet status to go to **Error** status with meaningful error messaging\. The phase in which fleet creation failed can also be a good indicator\. Fleet creation phases are: 
+ **New** – Fleet record is created\. Resources are allocated for the initial instance\.
+ **Downloading** – The game build files are downloaded to the instance and extracted\.
+ **Validating** – The downloaded game build files are validated\. 
+ **Building** – The game server build is installed on the instance, using an install script if available\.
+ **Activating** – Based on the fleet runtime configuration, server process\(es\) are started\. At least one process must communicate with Amazon GameLift to report its readiness to host a game session\.

As soon as one server process notifies Amazon GameLift that it is ready, the fleet status moves to **Active**\.

### Common Problems<a name="fleets-creating-debug-problems"></a>

**Downloading and validating**  
During this phase, fleet creation may fail if there are issues with the extracted build files, the installation script won't run, or if the executable\(s\) designated in the runtime configuration is not included in the build files\. Amazon GameLift provides logs related to each of these issues\.

If the logs do not reveal an issue, it's possible that the problem is due to an internal service error\. In this case, try to create the fleet again\. If the problem persists, consider re\-uploading the game build \(in case the files were corrupted\)\. You can also contact Amazon GameLift support or post a question on the forum\. 

**Building**  
Issues that cause failure during the build phase are almost certainly due to problems with the game build files and/or the installation script\. Verify that your game build files, as uploaded to Amazon GameLift, can be installed on a machine running the appropriate operating system\. Be sure to use a clean OS installation, not an existing development environment\. 

**Activating**  
The most common fleet creation problems occur during the **Activating** phase\. During this phase, a number of elements are being tested, including the game server's viability, the runtime configuration settings, and the game server's ability to interact with the Amazon GameLift service using the Server SDK\. Common issues that come up during fleet activation include: 

**Server processes fail to start\.**  
First check that you've correctly set the launch path and optional launch parameters in the fleet's runtime configuration\. You can view the fleet's current runtime configuration using either the Amazon GameLift console \(see the **Fleet** detail page, [**Capacity allocation**](gamelift-console-fleets-metrics.md#fleets-launch-tab)\) tab or by calling the AWS CLI command [describe\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-runtime-configuration.html)\. If the runtime configuration looks correct, check for issues with your game build files and/or installation script\.

**Server processes start but fleet fails to activate\.**  
If server processes start and run successfully, but the fleet does not move to **Active** status, a likely cause is that the server process is failing to notify Amazon GameLift that it is ready to host game sessions\. Check that your game server is correctly calling the Server API action `ProcessReady()` \(see [Prepare a Server Process](gamelift-sdk-server-api.md#gamelift-sdk-server-initialize)\)\.

**VPC peering connection request failed\.**  
For fleets that are created with a VPC peering connection \(see [Create a New Fleet with VPC Peering](vpc-peering.md#fleets-creating-aws-cli-vpc)\), VPC peering is done during this **Activating** phases\. If a VPC peering fails for any reason, the new fleet will fail to move to **Active** status\. You can track the success or failure of the peering request by calling [describe\-vpc\-peering\-connections](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-vpc-peering-connections.html)\. Be sure to check that a valid VPC peering authorization exists \([describe\-vpc\-peering\-authorizations](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-vpc-peering-authorizations.html), since authorizations are only valid for 24 hours\.

### Server Process Issues<a name="fleets-creating-debug-processes"></a>

**Server processes start but fail quickly or report poor health\.**  
Other than issues with your game build, this outcome can happen when trying to run too many server processes simultaneously on the instance\. The optimum number of concurrent processes depends on both the instance type and your game server's resource requirements\. Try reducing the number of concurrent processes, which is set in the fleet's runtime configuration, to see if performance improves\. You can change a fleet's runtime configuration using either the Amazon GameLift console \(edit the fleet's capacity allocation settings\) or by calling the AWS CLI command [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)\.

## Fleet Deletion Issues<a name="fleets-creating-debug-deletion"></a>

**Fleet can't be terminated due to max instance count\.**  
The error message indicates that the fleet being deleted still has active instances, which is not allowed\. You must first scale a fleet down to zero active instances\. This is done by manually setting the fleet's desired instance count to "0" and then waiting for the scale\-down to take effect\. Be sure to turn off auto\-scaling, which will counteract manual settings\. 

**VPC actions are not authorized\.**  
This issue only applies to fleets that you have specifically created VPC peering connections for \(see [VPC Peering for Amazon GameLift](vpc-peering.md)\. This scenario occurs because the process of deleting a fleet also includes deleting the fleet's VPC and any VPC peering connections\. You must first get an authorization by calling the GameLift service API [ CreateVpcPeeringAuthorization\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateVpcPeeringAuthorization.html) or use the AWS CLI command `create-vpc-peering-authorization`\. Once you have the authorization, you can delete the fleet\.

## Realtime Servers Fleet Issues<a name="fleets-creating-debug-realtime"></a>

**Zombie game sessions: They start and run a game, but they never end\.**  
You might observe this issues as any of the following scenarios:  
+ Script updates are not picked up by the fleet's Realtime servers\.
+ The fleet quickly reaches maximum capacity and does not scale down when player activity \(such as new game session requests\) decreases\. 
This is almost certainly a result of failing to successfully call `processEnding` in your Realtime script\. Although the fleet goes active and game sessions are started, there is no method for stopping them\. As a result, the Realtime server that is running the game session is never freed up to start a new one, and new game sessions can only start when new Realtime servers are spun up\. In addition, updates to the Realtime script do not impact already\- running game sessions, only ones\.   
To prevent this from happening, scripts need to provide a mechanism to trigger a `processEnding` call\. As illustrated in the [Realtime Servers Script Example](realtime-script.md#realtime-script-examples), one way is to program an idle session timeout where, if no player is connected for a certain amount of time, the script will end the current game session\.   
However, if you do fall into this scenario, there are a couple workarounds to get your Realtime servers unstuck\. The trick is to trigger the Realtime server processes—or the underlying fleet instances—to restart\. In this event, GameLift automatically closes the game sessions for you\. Once Realtime servers are freed up, they can start new game sessions using the latest version of the Realtime script\.   
There are a couple of methods to achieve this, depending on how pervasive the problem is:   
+ Scale the entire fleet down\. This method is the simplest to do but has a widespread effect\. Scale the fleet down to zero instances, wait for the fleet to fully scale down, and then scale it back up\. This will wipe out all existing game sessions, and let you start fresh with the most recently updated Realtime script\. 
+ Remotely access the instance and restart the process\. This is a good option if you have only a few processes to fix\. If you are already logged onto the instance, such as to tail logs or debug, then this may be the quickest method\. See [Remotely Access Fleet Instances](fleets-remote-access.md)\.

If you opt not to include way to call `processEnding` in your Realtime script, there are a couple of tricky situations that might occur even when the fleet goes active and game sessions are started\. First, a running game session does not end\. As a result, the server process that is running that game session is never free to start a new game session\. Second, the Realtime server does not pick up any script updates\. 