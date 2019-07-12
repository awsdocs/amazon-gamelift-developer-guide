# Run Multiple Processes on a Fleet<a name="fleets-multiprocess"></a>

This topic provides additional information on how to set up a fleet's runtime configuration to run multiple processes per instance\. Depending on how you configure your fleet, running multiple processes can give you greater control over your use of Amazon GameLift resources, improving efficiency and potentially reducing overall hosting costs\.

## Optimizing for Multiple Processes<a name="fleets-multiprocess-changes"></a>

At a minimum, you must do the following to enable multiple processes: 
+ [Create a build](gamelift-build-intro.md) that contains all of the game server executables that you want to deploy to a fleet and upload it to Amazon GameLift\. All game servers in a build must run on the same platform and be integrated with Amazon GameLift using the Amazon GameLift Server SDK for C\+\+, version 3\.0\.7 or later\. 
+ Create a runtime configuration with one or more server process configurations and multiple concurrent processes\.
+ Game clients connecting to games hosted on this fleet must be integrated using the AWS SDK, version 2016\-08\-04 or later\.

In addition, implement the following in your game servers to optimize fleet performance:
+ Handle server process shutdown scenarios to ensure that Amazon GameLift can recycle processes efficiently\. If you don't do this, server processes can't be shut down until they fail\.
  + Add a shutdown procedure to your game server code, ending with the server API call to `ProcessEnding()`\.
  + Implement the callback function `OnProcessTerminate()` in your game server code to gracefully handle termination requests from Amazon GameLift\.
+ Make sure that "unhealthy" server processes are shut down and relaunched quickly by defining what "healthy" and "unhealthy" mean and reporting this status back to Amazon GameLift\. You do this by implementing the `OnHealthCheck()` callback function in your game server code\. Amazon GameLift automatically shuts down server processes that are reported unhealthy for three consecutive minutes\. If you don't implement `OnHealthCheck()`, Amazon GameLift assumes a server process is healthy unless it fails to respond\. As a result, poorly performing server processes can continue to exist, using up resources until they finally fail\.

## How a Fleet Manages Multiple Processes<a name="fleets-multiprocess-howitworks"></a>

Amazon GameLift uses a fleet runtime configuration to manage what processes to maintain on each instance in the fleet\. A runtime configuration is actually made up of one or multiple server process configurations, each of which identifies the following: 
+ The path and file name of a server executable in the game build deployed on the fleet
+ \(Optional\) Parameters to pass to the server process on launch
+ The number of this server process to maintain concurrently on the instance

When an instance is started in the fleet, the instance immediately begins launching server processes called for in the runtime configuration\. Server process launches on an instance are staggered by a few seconds, so depending on the total number of server processes configured for an instance, it may take a few minutes to achieve full capacity\. 

Over time, server processes end, either by self\-terminating \(calling the Server SDK `ProcessEnding()`\) or by being terminated by Amazon GameLift\. An instance regularly checks that it is running the number and type of server processes specified in the runtime configuration\. If not, the instance automatically launches server processes as needed to meet the runtime configuration requirements\. As a result, as server processes end, their resources are continually recycled to support new server processes, and instances generally maintain the expected complement of active server processes\.

You can change a fleet's runtime configuration at any time by adding, changing, or removing server process configurations\. Here's how Amazon GameLift adopts runtime configuration changes\.

1. Before an instance checks that it is running the correct type and number of server processes, it first requests the latest version of the fleet's runtime configuration from the Amazon GameLift service\. If you've changed the runtime configuration, the instance acquires the new version and implements it\. 

1. The instance checks its active processes against the current runtime configuration and handles discrepancies as follows:
   + The updated runtime configuration removes a server process type\. Active server processes that no longer match the runtime configuration continue to run until they end\.
   + The updated runtime configuration decreases the number of concurrent processes for a server process type\. Excess server processes of that type continue to run until they end\.
   + The updated runtime configuration adds a new server process type or increases the concurrent processes setting for an existing type\. New server processes are launched immediately to match the updated runtime configuration, unless the instance is already running the maximum number of server processes\. In this case, new server processes are launched only when existing processes end\.

## Choosing the Number of Processes per Instance<a name="fleets-multiprocess-number"></a>

There are effectively three limits to keep in mind when deciding on the number of concurrent processes:
+  Amazon GameLift limits each instance to a [maximum number of concurrent processes](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_gamelift)\. Whether your runtime configuration has one or multiple server process configurations specified, the sum of all concurrent processes for a fleet's server process configurations can't exceed this limit\. 
+ The Amazon EC2 instance type that you choose may limit the number of processes that can run concurrently while offering acceptable performance levels\. You need to test different configurations for your game to find the optimal number of processes for your preferred instance type\. Factors that may affect your choice include the resource requirements of your game server, the number of players to be hosted in each game session, and player performance expectations\. 
+ When changing a fleet's runtime configuration, keep in mind that Amazon GameLift will never run more concurrent processes than the total number configured\. This means that the transition from old runtime configuration to new may happen gradually, with new processes starting only as existing processes end\. Here's an example: You have a fleet that is configured to run 10 concurrent processes of your server executable, `myGame.exe`, with launch parameters set to "\-loglevel=1"\. You update the configuration to continue running 10 concurrent processes of `myGame.exe` but change the launch parameters to "\-loglevel=4"\. Since instances in the fleet are already running 10 processes, Amazon GameLift waits to start a process with the new launch parameters until a process with the old launch parameters ends\.