# Logging server messages \(custom servers\)<a name="logging-server-messages-custom"></a>

You can capture custom server messages from your GameLift custom servers in log files\. To learn about logging for Realtime Servers, see [Logging server messages \(Realtime Servers\)](logging-server-messages-rts.md)\.

**Important**  
There is a limit on the size of a log file per game session \(see [Amazon GameLift endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/gamelift.html) in the *AWS General Reference*\)\. When a game session ends, GameLift uploads the server logs to Amazon Simple Storage Service \(Amazon S3\)\. GameLift will not upload logs that exceed the limit\. Logs can grow very quickly and exceed the size limit\. You should monitor your logs and limit the log output to necessary messages only\.

## Configuring logging for custom servers<a name="configuring-logging-for-custom-servers"></a>

With GameLift custom servers, you write your own code to perform logging, which you configure as part of your server process configuration\. GameLift uses your logging configuration to identify the files that it must upload to Amazon S3 at the end of each game session\.

The following instructions show how to configure logging using simplified code examples:

------
#### [ C\+\+ ]

**To configure logging \(C\+\+\)**

1. Create a vector of strings that are directory paths to game server log files\.

   ```
   std::string serverLog("serverOut.log");        // Example server log file
   std::vector<std::string> logPaths;
   logPaths.push_back(serverLog);
   ```

1. Provide your vector as the [LogParameters](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-log) of your [ProcessParameters](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-process) object\.

   ```
   Aws::GameLift::Server::ProcessParameters processReadyParameter = Aws::GameLift::Server::ProcessParameters(
       std::bind(&Server::onStartGameSession, this, std::placeholders::_1),
       std::bind(&Server::onProcessTerminate, this),
       std::bind(&Server::OnHealthCheck, this),
       std::bind(&Server::OnUpdateGameSession, this),
       listenPort,
       Aws::GameLift::Server::LogParameters(logPaths));
   ```

1. Provide the [ProcessParameters](integration-server-sdk-cpp-ref-datatypes.md#integration-server-sdk-cpp-ref-dataypes-process) object when you call [ProcessReady\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processready)\.

   ```
   Aws::GameLift::GenericOutcome outcome = 
      Aws::GameLift::Server::ProcessReady(processReadyParameter);
   ```

For a more complete example, see [ProcessReady\(\)](integration-server-sdk-cpp-ref-actions.md#integration-server-sdk-cpp-ref-processready)\.

------
#### [ C\# ]

**To configure logging \(C\#\)**

1. Create a list of strings that are directory paths to game server log files\.

   ```
   List<string> logPaths = new List<string>();
   logPaths.Add("C:\\game\\serverOut.txt");     // Example of a log file that the game server writes
   ```

1. Provide your list as the [LogParameters](integration-server-sdk-csharp-ref-datatypes.md#integration-server-sdk-csharp-ref-dataypes-log) of your [ProcessParameters](integration-server-sdk-csharp-ref-datatypes.md#integration-server-sdk-csharp-ref-dataypes-process) object\.

   ```
   var processReadyParameter = new ProcessParameters(
       this.OnGameSession,
       this.OnProcessTerminate,
       this.OnHealthCheck,
       this.OnGameSessionUpdate,
       port,
       new LogParameters(logPaths));
   ```

1. Provide the [ProcessParameters](integration-server-sdk-csharp-ref-datatypes.md#integration-server-sdk-csharp-ref-dataypes-process) object when you call [ProcessReady\(\)](integration-server-sdk-csharp-ref-actions.md#integration-server-sdk-csharp-ref-processready)\.

   ```
   var processReadyOutcome =
      GameLiftServerAPI.ProcessReady(processReadyParameter);
   ```

For a more complete example, see [ProcessReady\(\)](integration-server-sdk-csharp-ref-actions.md#integration-server-sdk-csharp-ref-processready)\.

------

## Writing to logs<a name="writing-to-logs-for-custom-servers"></a>

Your log files exist after your server process has started\. You can write to the logs using any method to write to files\. To capture all of your server's standard output and error output, remap the output streams to log files, as in the following examples:

------
#### [ C\+\+ ]

```
std::freopen("serverOut.log", "w+", stdout);
std::freopen("serverErr.log", "w+", stderr);
```

------
#### [ C\# ]

```
Console.SetOut(new StreamWriter("serverOut.txt"));
Console.SetError(new StreamWriter("serverErr.txt"));
```

------

## Accessing server logs<a name="accessing-logs-for-custom-servers"></a>

When a game session ends, GameLift automatically stores the logs in an Amazon S3 bucket and retains them for a limited period of time\. To get the location of the logs for a game session, you can use the [GetGameSessionLogUrl](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetGameSessionLogUrl.html) API operation\. To download the logs, use the URL that the operation returns\.