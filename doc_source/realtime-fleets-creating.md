# Deploy a Realtime Servers Fleet<a name="realtime-fleets-creating"></a>

You can create a new fleet of Realtime game servers to host game sessions for your game\. Realtime Servers fleets require that you create a Realtime script and upload it to Amazon GameLift\. If you have a custom game server build, see [Deploy a GameLift Fleet for a Custom Game Build](fleets-creating.md) for help creating a fleet with it\. Use either the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/) or the AWS Command Line Interface \(CLI\) to create a fleet\. You can change a fleet's configuration by [editing a fleet](fleets-editing.md)\.

## Create a Realtime Fleet \(Console\)<a name="realtime-fleets-creating-console"></a>

**To create a Realtime fleet with the Amazon GameLift console:**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\. Go to the **Fleets: Create fleet** page to configure a new fleet\. 

1. **Fleet Details**\.
   + **Name** – Create a meaningful fleet name so you can easily identify it in a list and in metrics\.
   + **Description** – \(Optional\) Add a short description for this fleet to further aid identification\.
   + **Fleet type** – Choose whether to use on\-demand or spot instances for this fleet\. Learn more about fleet types in [Choose Computing Resources](gamelift-ec2-instances.md)\.
   + **Metric group** – \(Optional\) Enter the name of a new or existing fleet metric group\. When using Amazon CloudWatch to track your Amazon GameLift metrics, you can aggregate the metrics for multiple fleets by adding them to the same metric group\.
   + **Instance role ARN** – \(Optional\) Enter the ARN value for an IAM role that you want to associated with this fleet\. This setting allows all instances in the fleet to assume the role, which extends access to a defined set of AWS services\. Learn more about how to [Access AWS Resources From Your Fleets](gamelift-sdk-server-resources.md)\.
   + **Certificate type **– Choose whether to have GameLift generate a TLS certificate for the fleet\. With this feature enabled for a Realtime fleet, GameLift automatically authenticates the client/server connection and encrypts all communication between game client and server\. Once the fleet is created, you cannot change the certificate type\.
   + **Binary type** – Select the binary type "Script"\.
   + **Script** – Select the Realtime script you want to deploy from the dropdown list\.

1. **Instance type**\. Select an Amazon EC2 instance type from the list\. The instance types listed vary depending several factors, including the current region, the operating system of the selected game build, and the fleet type \(on\-demand or spot\)\. Learn more about choosing an instance type in [Choose Computing Resources](gamelift-ec2-instances.md)\. Once the fleet is created, you cannot change the instance type\.

1. **Process management**\. Configure how you want server processes to run on each instance\.

   1. 

**Server process allocation\.**

      Specify the type and number of game server processes you want to run on each instance\. Each fleet must have at least one server process configuration defined and can have multiple configurations\. For example, if you want to launch processes using different files in your uploaded Realtime script, you must have a configuration for each type of process you want to launch\. 
      + **Launch path** – Type the name of the script file that you want to launch with\. A launch script file must call an `Init()` function\. During deployment, your uploaded Realtime script files are unzipped and stored in the /local/game/ directory, so you just need to specify the script file name\. Example: **MyRealtimeLaunchScript\.js**\.
      + **Launch parameters** – \(Optional\) You can pass information to your Realtime script at launch time\. Type the information as a set of command line parameters\. Example: **\+map Winter444**\.
      + **Concurrent processes** – Indicate how many server processes with this configuration to run concurrently on each instance in the fleet\. 

      Once you enter a server process configuration, click the green checkmark button on the right to save the configuration\. To add additional server process configurations, click **Add configuration**\. 

      Check the Amazon GameLift [limits](https://aws.amazon.com//ec2/faqs/#Is_Amazon_EC2_used_in_conjunction_with_Amazon_S3) on the number of concurrent server processes\. Limits on concurrent server processes per instance apply to the total of concurrent processes set for all configurations\. For example, if you're limited to one process, you can have only one configuration, and concurrent processes must be set to **1**\. If a fleet is configured to exceed the limit, the fleet cannot activate\.

      The collection of server process configurations is called the fleet's runtime configuration\. It describes all server processes that will be running on each instance in this fleet at any given time\. 

   1. 

**Game session activation \(optional\):**

      Set the following limits to determine how new game sessions are activated on the instances in this fleet:
      + **Max concurrent game session activation** – Limit the number of game sessions on an instance that can be activating at the same time\. This limit is useful when launching multiple new game sessions may have an impact on the performance of other game sessions running on the instance\.
      + **New activation timeout** – This setting limits the amount of time Amazon GameLift allows for a new game session to activate\. If the game session does not move to status ACTIVE, the game session activation process is terminated\. 

1. **Protection policy \(optional\)**\. Indicate whether or not to apply game session protection to instances in this fleet\. Protected instances are not terminated during a scale\-down event if they are hosting an active game session\. Using this setting applies a fleet\-wide protection policy; you can also set protection for individual game sessions when creating the game session\. 

1. Once you've finished configuring the new fleet, click **Initialize fleet**\. Amazon GameLift assigns an ID to the new fleet and begins the fleet activation process\. You can view the new fleet's status on the **Fleets** page\. Once the fleet is active, you can [change the fleet's capacity](fleets-updating-capacity.md), runtime configuration, and other configuration settings as needed\.

## Create a Realtime Fleet \(AWS CLI\)<a name="realtime-fleets-creating-aws-cli"></a>

To create a Realtime fleet with the AWS CLI, open a command line window and use the `create-fleet` command to define a new fleet\. See complete documentation on this command in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

The example `create-fleet` request shown below creates a new fleet with the following characteristics: 
+ The fleet will use c4\.large spot instances\. 
+ It will deploy the specified Realtime script\.
+ Each instance in the fleet will run ten identical processes of the Realtime script concurrently, enabling each instance to host up to ten game sessions simultaneously\.
+ On each instance, Amazon GameLift will allow only two new game sessions to be activating at the same time\. It will also terminate any activating game session if it is not ready to host players within 60 seconds\.
+ All game sessions hosted on instances in this fleet have game session protection turned on\. It can be turned off for individual game sessions\. 
+ Individual players can create three new game sessions within a 15\-minute period\.
+ Metrics for this fleet will be added to the EMEAfleets metric group, which \(in this example\) combines metrics for all fleets in EMEA regions\. 

**Note**  
For Realtime Servers fleets, Amazon GameLift automatically sets TCP and UDP ranges for use by the Realtime servers\. You can view the automatic settings by calling the CLI command `describe-fleet-port-settings`\. 

```
$ aws gamelift create-fleet
    --name "SampleRealtimeFleet123"
    --description "A sample Realtime fleet"
    --ec2-instance-type "c4.large"
    --fleet-type "SPOT"
    --script-id "script-1111aaaa-22bb-33cc-44dd-5555eeee66ff"
    --certificate-configuration "CertificateType=GENERATED"
    --runtime-configuration "GameSessionActivationTimeoutSeconds=60,
                             MaxConcurrentGameSessionActivations=2,
                             ServerProcesses=[{LaunchPath=/local/game/myRealtimeLaunchScript.js,
                                               Parameters=+map Winter444,
                                               ConcurrentExecutions=10}]"
    --new-game-session-protection-policy "FullProtection"
    --resource-creation-limit-policy "NewGameSessionsPerCreator=3,
                                      PolicyPeriodInMinutes=15"
    --MetricGroups "EMEAfleets"
```

*Copiable version:*

```
aws gamelift create-fleet --name "SampleRealtimeFleet123" --description "A sample Realtime fleet" --ec2-instance-type "c4.large" --fleet-type "SPOT" --script-id "script-1111aaaa-22bb-33cc-44dd-5555eeee66ff" --certificate-configuration "CertificateType=GENERATED" --runtime-configuration "GameSessionActivationTimeoutSeconds=60,MaxConcurrentGameSessionActivations=2,ServerProcesses=[{LaunchPath=/local/game/myRealtimeLaunchScript.js,Parameters=+map Winter444,ConcurrentExecutions=10}]" --new-game-session-protection-policy "FullProtection" --resource-creation-limit-policy "NewGameSessionsPerCreator=3,PolicyPeriodInMinutes=15" --MetricGroups "EMEAfleets"
```

If the create\-fleet request is successful, Amazon GameLift returns a set of fleet attributes that includes the configuration settings you requested and a new fleet ID\. Amazon GameLift immediately initiates the fleet activation process and sets the fleet status to **New**\. You can track the fleet's status and view other fleet information using these CLI commands: 
+ [describe\-fleet\-events](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-events.html)
+ [describe\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-attributes.html)
+ [describe\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-capacity.html)
+ [describe\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-port-settings.html)
+ [describe\-fleet\-utilization](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-utilization.html)
+ [describe\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-runtime-configuration.html)

Once the fleet is active, you can change the fleet's capacity and other configuration settings as needed using these commands:
+ [update\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-attributes.html)
+ [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html)
+ [update\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html)
+ [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)