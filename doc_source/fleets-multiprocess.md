# Manage how GameLift launches game servers<a name="fleets-multiprocess"></a>

You can set up an managed EC2 fleet's runtime configuration to run multiple game server processes per instance\. This uses your hosting resources more efficiently\.

## How a fleet manages multiple processes<a name="fleets-multiprocess-howitworks"></a>

Amazon GameLift uses a fleet's runtime configuration to determine the type and number of processes to run on each instance\. A runtime configuration contains at least one server process configuration that represents one game server executable\. You can define additional server process configurations to run other types of processes related to your game\. Each server process configuration contains the following information:
+ The file name and path of an executable in your game build\.
+ \(Optional\) Parameters to pass to the process on launch\.
+ The number of processes to run concurrently\.

When an instance in the fleet activates, it launches the set of server processes defined in the runtime configuration\. With multiple processes, GameLift staggers the launch of each process\. Server processes have a limited life span\. As they end, GameLift launches new processes to maintain the number and type of server processes defined in the runtime configuration\.

You can change the runtime configuration at any time by adding, changing, or removing server process configurations\. Each instance regularly checks for updates to the fleet's runtime configuration to implement the changes\. Here's how GameLift adopts runtime configuration changes:

1. The instance sends a request to GameLift for the latest version of the runtime configuration\.

1. The instance compares its active processes to the latest runtime configuration, and then does the following:
   + If the updated runtime configuration removes a server process type, then active server processes of this type continue to run until they end\. The instance doesn't replace these server processes\.
   + If the updated runtime configuration decreases the number of concurrent processes for a server process type, then excess server processes of this type continue to run until they end\. The instance doesn't replace these excess server processes\.
   + If the updated runtime configuration adds a new server process type or increases the concurrent processes for an existing type, then the instance starts new server processes, up to the GameLift maximum\. In this case, the instance launches new server processes as existing processes end\.

## Optimize a fleet for multiple processes<a name="fleets-multiprocess-changes"></a>

To use multiple processes on a fleet, do the following:
+ [Create a build](gamelift-build-intro.md) that contains the game server executables that you want to deploy to a fleet, and then upload the build to GameLift\. All game servers in a build must run on the same platform and use the GameLift Server SDK\.
+ Create a runtime configuration with one or more server process configurations and multiple concurrent processes\.
+ Integrate game clients with the AWS SDK version 2016\-08\-04 or later\.

To optimize fleet performance, we recommend that you do the following:
+ Handle server process shutdown scenarios so that GameLift can recycle processes efficiently\. For example:
  + Add a shutdown procedure to your game server code that calls the server API `ProcessEnding()`\.
  + Implement the callback function `OnProcessTerminate()` in your game server code to handle termination requests from GameLift\.
+ Make sure that GameLift shuts down and relaunches unhealthy server processes\. Report the health status back to GameLift by implementing the `OnHealthCheck()` callback function in your game server code\. GameLift automatically shuts down server processes that are reported unhealthy for three consecutive reports\. If you don't implement `OnHealthCheck()`, then GameLift assumes that a server process is healthy, unless the process fails to respond to a communication\.

## Choose the number of processes per instance<a name="fleets-multiprocess-number"></a>

When deciding on the number of concurrent processes to run on an instance, keep in mind the following:
+ GameLift limits each instance to a [maximum number of concurrent processes](https://docs.aws.amazon.com/general/latest/gr/gamelift.html#limits_gamelift)\. The sum of all concurrent processes for a fleet's server process configurations can't exceed this quota\.
+ To maintain acceptable performance levels, the Amazon EC2 instance type might limit the number of processes that can run concurrently\. Test different configurations for your game to find the right number of processes for your preferred instance type\.
+ GameLift doesn't run more concurrent processes than the total number configured\. This means that the transition from the previous runtime configuration to the new configuration might happen gradually\.