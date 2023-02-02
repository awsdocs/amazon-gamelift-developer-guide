# Logging server messages \(Realtime Servers\)<a name="logging-server-messages-rts"></a>

You can capture custom server messages from your Realtime Servers in log files\. To learn about logging for custom servers, see [Logging server messages \(custom servers\)](logging-server-messages-custom.md)\.

There are different types of messages that you can ouptput to your log files \(see [Logging messages in your server script](#logging-rts-messages-in-server-script)\)\. In addition to your custom messages, your Realtime Servers output system messages using the same message types and write to the same log files\. You can adjust the logging level for your fleet to reduce the amount of logging messages that your servers generate \(see [Adjusting the logging level](#adjusting-rts-logging-level)\)\.

**Important**  
There is a limit on the size of a log file per game session \(see [Amazon GameLift endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/gamelift.html) in the *AWS General Reference*\)\. When a game session ends, GameLift uploads the server logs to Amazon Simple Storage Service \(Amazon S3\)\. GameLift will not upload logs that exceed the limit\. Logs can grow very quickly and exceed the size limit\. You should monitor your logs and limit the log output to necessary messages only\.

## Logging messages in your server script<a name="logging-rts-messages-in-server-script"></a>

You can output custom messages in the [script for your Realtime Servers](realtime-script.md)\. Use the following steps to send server messages to a log file:

1. Create a varaible to hold the reference to the logger object\.

   ```
   var logger;
   ```

1. In the `init()` function, get the logger from the session object and assign it to your logger variable\.

   ```
   function init(rtSession) {
       session = rtSession;
       logger = session.getLogger();
   }
   ```

1. Call the appropriate function on the logger to output a message\.

   **Debug messages**

   ```
   logger.debug("This is my debug message...");
   ```

   **Informational messages**

   ```
   logger.info("This is my info message...");
   ```

   **Warning messages**

   ```
   logger.warn("This is my warn message...");
   ```

   **Error messages**

   ```
   logger.error("This is my error message...");
   ```

   **Fatal error messages**

   ```
   logger.fatal("This is my fatal error message...");
   ```

   **Customer experience fatal error messages**

   ```
   logger.cxfatal("This is my customer experience fatal error message...");
   ```

For an example of the logging statements in a script, see [Realtime Servers script example](realtime-script.md#realtime-script-examples)\.

The output in the log files indicates the type of message \(`DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`, `CXFATAL`\), as shown in the following lines from an example log:

```
09 Sep 2021 11:46:32,970 [INFO] (gamelift.js) 215: Calling GameLiftServerAPI.InitSDK...
09 Sep 2021 11:46:32,993 [INFO] (gamelift.js) 220: GameLiftServerAPI.InitSDK succeeded
09 Sep 2021 11:46:32,993 [INFO] (gamelift.js) 223: Waiting for Realtime server to start...
09 Sep 2021 11:46:33,15 [WARN] (index.js) 204: Connection is INSECURE. Messages will be sent/received as plaintext.
```

## Accessing server logs<a name="accessing-rts-server-logs"></a>

When a game session ends, GameLift automatically stores the logs in Amazon S3 and retains them for a limited period of time\. You can use the [GetGameSessionLogUrl API call](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetGameSessionLogUrl.html) to get the locaion of the logs for a game session\. Use URL returned by the API call to download the logs\.

## Adjusting the logging level<a name="adjusting-rts-logging-level"></a>

Logs can grow very quickly and exceed the size limit\. You should monitor your logs and limit the log output to necessary messages only\. For Realtime Servers, you can adjust the logging level by providing a parameter in your fleet's runtime configuration in the form `loggingLevel:LOGGING_LEVEL`, where `LOGGING_LEVEL` is one of the following values:

1. `debug`

1. `info` *\(default\)*

1. `warn`

1. `error`

1. `fatal`

1. `cxfatal`

This list is ordered from least severe \(`debug`\) to most severe \(`cxfatal`\)\. You set a single `loggingLevel` and the server will only log messages at that severity level or a higher severity level\. For example, setting `loggingLevel:error` will make all of the servers in your fleet only write `error`, `fatal`, and `cxfatal` messages to the log\.

You can set the logging level for your fleet when you create it or after it is running\. Changing your fleet's logging level after it is running will only affect logs for game sessions created after the update\. Logs for any exisitng game sessions won't be affected\. If you don't set a logging level when you create your fleet, your servers will set the logging level to `info` by default\. Refer to the following sections for instructions to set the logging level\.

**Setting the logging level when creating a Realtime Servers fleet \(Console\)**  
Follow the instructions at [Create a managed fleet](fleets-creating.md) to create your fleet, with the following addition:
+ In the **Server process allocation** substep of the **Process management** step, provide the logging level key\-value pair \(such as `loggingLevel:error`\) as a value for **Launch parameters**\. Use a non\-alphanumeric character \(except comma\) to separate the logging level from any additional parameters \(for example, `loggingLevel:error +map Winter444`\)\.

**Setting the logging level when creating a Realtime Servers fleet \(AWS CLI\)**  
Follow the instructions at [Create a managed fleet](fleets-creating.md) to create your fleet, with the following addition:
+ In the argument to the `--runtime-configuration` parameter for `[create\-fleet](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet.html)`, provide the logging level key\-value pair \(such as `loggingLevel:error`\) as a value for `Parameters`\. Use a non\-alphanumeric character \(except comma\) to separate the logging level from any additional parameters\. See the following example:

```
--runtime-configuration "GameSessionActivationTimeoutSeconds=60,
                    MaxConcurrentGameSessionActivations=2,
                    ServerProcesses=[{LaunchPath=/local/game/myRealtimeLaunchScript.js,
                                    Parameters=loggingLevel:error +map Winter444,
                                    ConcurrentExecutions=10}]"
```

**Setting the logging level for a running Realtime Servers fleet \(Console\)**  
Follow the instructions at [Update a fleet configuration](fleets-editing.md#fleets-update) to update your fleet using the GameLift Console, with the following addition:
+ On the **Edit fleet** page, under **Server process allocation**, provide the logging level key\-value pair \(such as `loggingLevel:error`\) as a value for **Launch parameters**\. Use a non\-alphanumeric character \(except comma\) to separate the logging level from any additional parameters \(for example, `loggingLevel:error +map Winter444`\)\.

**Setting the logging level for a running Realtime Servers fleet \(AWS CLI\)**  
Follow the instructions at [Update a fleet configuration](fleets-editing.md#fleets-update) to update your fleet using the AWS CLI, with the following addition:
+ In the argument to the `--runtime-configuration` parameter for `[update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)`, provide the logging level key\-value pair \(such as `loggingLevel:error`\) as a value for `Parameters`\. Use a non\-alphanumeric character \(except comma\) to separate the logging level from any additional parameters\. See the following example:

```
--runtime-configuration "GameSessionActivationTimeoutSeconds=60,
                    MaxConcurrentGameSessionActivations=2,
                    ServerProcesses=[{LaunchPath=/local/game/myRealtimeLaunchScript.js,
                                    Parameters=loggingLevel:error +map Winter444,
                                    ConcurrentExecutions=10}]"
```