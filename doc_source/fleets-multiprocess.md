# Managing how game servers are launched for hosting<a name="fleets-multiprocess"></a>

You can set up a fleet's runtime configuration to run multiple game server processes per instance\. By running multiple processes on each instance can help you use your hosting resources more efficiently and potentially reduce overall hosting costs\.

## How a fleet manages multiple processes<a name="fleets-multiprocess-howitworks"></a>

GameLift uses a fleet's runtime configuration to determine the type and number of processes to run on each instance in the fleet\. At a minimum, a runtime configuration contains one server process configuration that represents one game server executable\. You can also define additional server process configurations to run other types of processes related to your game\. Each server process configuration contains the following information: 
+ The file name and path of an executable in your game build\.
+ \(Optional\) Parameters to pass to the process on launch\.
+ The number of processes to run concurrently\.

When an instance in the fleet is activated, it immediately launches the set of server processes that are defined in the runtime configuration\. With multiple processes, each process launch is staggered by a few seconds, so instances with many processes might take a few minutes to achieve full activation\. Processes have a limited life span\. As they terminate, instances continual launch new processes in order to maintain the number and type of server processes as defined in the current runtime configuration\.

You can change the runtime configuration at any time by adding, changing, or removing server process configurations\. Each instance regularly checks for updates to the fleet's runtime configuration, ensuring that changes are implemented quickly\. Here's how GameLift adopts runtime configuration changes\.

1. Before an instance checks that it is running the correct type and number of server processes, the instance sends a request to the GameLift service for the latest version of the runtime configuration\. 

1. The instance checks its active processes against the latest runtime configuration and handles updates as follows:
   + If the updated runtime configuration removes a server process type: Active server processes of this type continue to run until they end, and are not replaced when they terminate\.
   + If the updated runtime configuration decreases the number of concurrent processes for a server process type: Excess server processes of this type continue to run until they end, and are not replaced when they terminate\.
   + If the updated runtime configuration adds a new server process type or increases the concurrent processes for an existing type: New server processes are started immediately, up to the GameLift maximum for server processes on an instance\. In this case, new server processes are launched only as existing processes end\.

## Optimizing a fleet for multiple processes<a name="fleets-multiprocess-changes"></a>

At a minimum, you must do the following to enable multiple processes: 
+ [Create a build](gamelift-build-intro.md) that contains all of the game server executables that you want to deploy to a fleet and upload it to GameLift\. All game servers in a build must run on the same platform and be integrated with GameLift using the GameLift Server SDK for C\+\+, version 3\.0\.7 or later\.
+ Create a runtime configuration with one or more server process configurations and multiple concurrent processes\.
+ Game clients that connect to games hosted on this fleet must be integrated using the AWS SDK version 2016\-08\-04 or later\.

In addition, follow these game server integration recommendations to optimize fleet performance:
+ Handle server process shutdown scenarios to ensure that GameLift can recycle processes efficiently\. Without these, server processes won't shut down until they fail, and runtime configuration updates may be slow to implement\.
  + Add a shutdown procedure to your game server code that calls server API `ProcessEnding()`\. 
  + Implement the callback function `OnProcessTerminate()` in your game server code to gracefully handle termination requests from GameLift\.
+ Make sure that unhealthy server processes are shut down and relaunched quickly\. Define what "healthy" and "unhealthy" mean for your game and maintain each process's health status\. Report this status back to GameLift by implementing the `OnHealthCheck()` callback function in your game server code\. GameLift automatically shuts down server processes that are reported unhealthy for three consecutive reports\. If you don't implement `OnHealthCheck()`, GameLift assumes that a server process is healthy unless the process fails to respond to a communication\. As a result, poorly performing server processes can continue to exist, using up resources until they finally fail\.

## Choosing the number of processes per instance<a name="fleets-multiprocess-number"></a>

When deciding on the number of concurrent processes to run on an instance, there are three limits to keep in mind:
+  GameLift limits each instance to a [maximum number of concurrent processes](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_gamelift)\. Whether your runtime configuration has one or multiple server process configurations specified, the sum of all concurrent processes for a fleet's server process configurations can't exceed this limit\. 
+ The Amazon EC2 instance type that you choose may limit the number of processes that can run concurrently in order to maintain acceptable performance levels\. You need to test different configurations for your game to find the optimal number of processes for your preferred instance type\. Factors that may affect your choice include the resource requirements of your game server, the number of players to be hosted in each game session, and player performance expectations\. 
+ When changing a fleet's runtime configuration, keep in mind that GameLift will never run more concurrent processes than the total number configured\. This means that the transition from old runtime configuration to new might happen gradually, with new processes starting only as existing processes end\. For example, say you have a fleet that is configured to run 10 concurrent processes of your server executable, `myGame.exe`, with launch parameters set to "\-loglevel=1"\. You update the configuration to continue running 10 concurrent processes of `myGame.exe` but change the launch parameters to "\-loglevel=4"\. Because instances in the fleet are already running 10 processes, each instance cannot start a process with the new launch parameters until a process with the old launch parameters ends\.