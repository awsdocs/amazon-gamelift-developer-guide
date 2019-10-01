# Deploy a GameLift Fleet for a Custom Game Build<a name="fleets-creating"></a>

If you're using Realtime Servers for your game, see [Deploy a Realtime Servers Fleet](realtime-fleets-creating.md)\.

You can create and deploy a new fleet to host game servers for any game build that has been uploaded to the Amazon GameLift service and is in a **Ready** status\. <a name="fleets-creating-builds-fleets-page"></a><a name="fleets-creating-aws-cli"></a>

**Create a fleet**  
Use either the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/) or the AWS Command Line Interface \(CLI\) to create a fleet\. 

After you create a new fleet, the fleet's status passes through several stages as the fleet is deployed and game servers installed and started up\. Once the fleet reaches ACTIVE status, it is ready to host game sessions\. For help with fleet creation issues, see [Debug Fleet Issues](fleets-creating-debug.md)\.

------
#### [ Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Builds** page, find the build that you want to create a fleet for and verify that its status is **Ready**\. Select the build \(use the option button to the left of the build status\) and click **Create fleet from build**\.

1. On the **Create fleet** page, enter the **Fleet Details**: 
   + **Name** – Create a meaningful fleet name so you can easily identify it in a list and in metrics\.
   + **Description** – \(Optional\) Add a short description for this fleet to further aid identification\.
   + **Fleet type** – Choose whether to use on\-demand or spot instances for this fleet\. Learn more about fleet types in [Choose Computing Resources](gamelift-ec2-instances.md)\.
   + **Metric group** – \(Optional\) Enter the name of a new or existing fleet metric group\. When using Amazon CloudWatch to track your Amazon GameLift metrics, you can aggregate the metrics for multiple fleets by adding them to the same metric group\.
   + **Instance role ARN** – \(Optional\) Enter the ARN value for an IAM role that you want to associated with this fleet\. This setting allows all instances in the fleet to assume the role, which extends access to a defined set of AWS services\. Learn more about how to [Access AWS Resources From Your Fleets](gamelift-sdk-server-resources.md)\.
   + **Certificate type** – Choose whether to have GameLift generate a TLS certificate for the fleet\. You can use a fleet TLS certificate to have your game client authenticate a game server when connecting, and encrypt all client/server communication\. For each instance in a TLS\-enabled fleet, GameLift also creates a new DNS entry with the certificate\. Use these resources to set up authentication and encryption for your game\. Once the fleet is created, you cannot change the certificate type\.
   + **Binary type** – Select the binary type "Build"\. 
   + **Build** – If you used the **Create fleet from build feature**, the build information, including name, ID and operating system, is automatically filled in\. Otherwise, select a valid build from the dropdown list\.

1. **Instance type**\. Select an Amazon EC2 instance type from the list\. The instance types listed vary depending several factors, including the current region, the operating system of the selected game build, and the fleet type \(on\-demand or spot\)\. Learn more about choosing an instance type in [Choose Computing Resources](gamelift-ec2-instances.md)\. Once the fleet is created, you cannot change the instance type\.

1. **Process management**\. Configure how you want server processes to run on each instance\.

   1. 

**Server process allocation:**

      Specify the type and number of game server processes you want to run on each instance\. Each fleet must have at least one server process configuration defined and can have multiple configurations\. For example, if your game build has multiple server executables, you must have a configuration for each executable\. 
      + **Launch path** – Type the path to the game executable in your build\. All launch paths must start with the game server location, which varies based on the operating system in use\. On Windows instances, game servers are built to the path `C:\game`\. On Linux instances, game servers are built to `/local/game`, so all launch paths must start with this location\. Examples: **C:\\game\\MyGame\\server\.exe** or **/local/game/MyGame/server\.exe**\.
      + **Launch parameters** – \(Optional\) You can pass information to your game executable at launch time\. Type the information as a set of command line parameters here\. Example: **\+sv\_port 33435 \+start\_lobby**\.
      + **Concurrent processes** – Indicate how many server processes with this configuration to run concurrently on each instance in the fleet\. Check the Amazon GameLift [limits](https://aws.amazon.com//ec2/faqs/#Is_Amazon_EC2_used_in_conjunction_with_Amazon_S3) on number of concurrent server processes; they depend on which SDK your game server uses\. 

      Once you enter a server process configuration, click the green checkmark button to save the configuration\. To add additional server process configurations, click **Add configuration**\. 

      Limits on concurrent server processes per instance apply to the total of concurrent processes for all configurations\. If you're limited to one process, you can have only one configuration, and concurrent processes must be set to **1**\. If you configure the fleet to exceed the limit, the fleet cannot activate\.

      The collection of server process configurations is called the fleet's runtime configuration\. It describes all server processes that will be running on each instance in this fleet at any given time\. 

   1. 

**Game session activation:**

      Set the following limits to determine how new game sessions are activated on the instances in this fleet:
      + **Max concurrent game session activation** – Limit the number of game sessions on an instance that can be activating at the same time\. This limit is useful when launching multiple new game sessions may have an impact on the performance of other game sessions running on the instance\.
      + **New activation timeout** – This setting limits the amount of time Amazon GameLift allows for a new game session activate\. If the game session does not complete activation and move to status ACTIVE, the game session activation is terminated\. 

1. **EC2 port settings**\. Click **Add port settings** to define access permissions for inbound traffic connecting to server processes deployed on this fleet\. You can create multiple port settings for a fleet\. At least one port setting must be set for the fleet before access is allowed\. If you don't specify port settings at this time, you can edit the fleet later\.
   + **Port range** – Specify a range of port numbers that your game servers can use to allow inbound connections\. A port range must use the format `nnnnn[-nnnnn]`, with values between 1025 and 60000\. Example: **1500** or **1500\-20000**\. 
   + **Protocol** – Select the type of communication protocol for the fleet to use\.
   + **IP address range** – Specify a range of IP addresses valid for instances in this fleet\. Use CIDR notation\. Example: **0\.0\.0\.0/0** \(This example allows access to anyone trying to connect\.\)

1. In the **Protection policy** section, choose whether to apply game session protection to instances in this fleet\. Instances with protection are not terminated during a scale down event if they are hosting an active game session\. You can also set protection for individual game sessions\. Once the fleet is created, you can edit the fleet to change the fleet\-wide protection policy\.

1. Once you've finished setting the configuration for your new fleet, click **Initialize fleet**\. Amazon GameLift assigns an ID to the new fleet and begins the fleet activation process\. You can track the new fleet's status on the **Fleets** page\. 

You can update the fleet's metadata and configuration at any time, regardless of fleet status \(see [Manage Fleet Records](fleets-editing.md)\)\. You can update fleet capacity only once the fleet has reached ACTIVE status \(see [Scaling Amazon GameLift Fleet Capacity](fleets-manage-capacity.md)\)\.

------
#### [ AWS CLI ]

To create a fleet with the AWS CLI, open a command line window and use the `create-fleet` command to define a new fleet\. See complete documentation on this command in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

The example `create-fleet` request shown below creates a new fleet with the following characteristics: 
+ The fleet will use c4\.large on\-demand instances with the operating system required for the selected game build\. 
+ It will deploy the specified game server build, which must be in a **Ready** status\.
+ TLS certificate generation is enabled\.
+ Each instance in the fleet will run ten identical processes of the game server concurrently, enabling each instance to host up to ten game sessions simultaneously\.
+ On each instance, Amazon GameLift will allow only two new game sessions to be activating at the same time\. It will also terminate any activating game session if it is not ready to host players within 300 seconds\.
+ All game sessions hosted on instances in this fleet have game session protection turned on\. It can be turned off for individual game sessions\. 
+ Individual players can create three new game sessions within a 15\-minute period\.
+ Each game session hosted on this fleet will have a connection point that falls within the specified IP address and port ranges\.
+ Metrics for this fleet will be added to the EMEAfleets metric group, which \(in this example\) combines metrics for all fleets in EMEA regions\. 

```
$ aws gamelift create-fleet
    --name "SampleFleet123"
    --description "The sample test fleet"
    --ec2-instance-type "c4.large"
    --fleet-type "ON_DEMAND"
    --build-id "build-92f061ed-27c9-4a02-b1f4-6f85b2385620"
    --certificate-configuration "CertificateType=GENERATED"
    --runtime-configuration "GameSessionActivationTimeoutSeconds=300,
                             MaxConcurrentGameSessionActivations=2,
                             ServerProcesses=[{LaunchPath=C:\game\Bin64.dedicated\MultiplayerSampleProjectLauncher_Server.exe,
                                               Parameters=+sv_port 33435 +start_lobby,
                                               ConcurrentExecutions=10}]"
    --new-game-session-protection-policy "FullProtection"
    --resource-creation-limit-policy "NewGameSessionsPerCreator=3,
                                      PolicyPeriodInMinutes=15"
    --ec2-inbound-permissions "FromPort=33435,ToPort=33435,IpRange=0.0.0.0/0,Protocol=UDP" 
                              "FromPort=33235,ToPort=33235,IpRange=0.0.0.0/0,Protocol=UDP"
    --MetricGroups "EMEAfleets"
```

*Copiable version:*

```
aws gamelift create-fleet --name "SampleFleet123" --description "The sample test fleet" --fleet-type "ON_DEMAND" --MetricGroups "EMEAfleets" --build-id "build-92f061ed-27c9-4a02-b1f4-6f85b2385620" --certificate-configuration "CertificateType=GENERATED" --ec2-instance-type "c4.large" --runtime-configuration "GameSessionActivationTimeoutSeconds=300,MaxConcurrentGameSessionActivations=2,ServerProcesses=[{LaunchPath=C:\game\Bin64.dedicated\MultiplayerSampleProjectLauncher_Server.exe,Parameters=+sv_port 33435 +start_lobby,ConcurrentExecutions=10}]" --new-game-session-protection-policy "FullProtection" --resource-creation-limit-policy "NewGameSessionsPerCreator=3,PolicyPeriodInMinutes=15" --ec2-inbound-permissions "FromPort=33435,ToPort=33435,IpRange=0.0.0.0/0,Protocol=UDP" "FromPort=33235,ToPort=33235,IpRange=0.0.0.0/0,Protocol=UDP"
```

If the create\-fleet request is successful, Amazon GameLift returns a set of fleet attributes that includes the configuration settings you requested and a new fleet ID\. Amazon GameLift immediately initiates the fleet activation process and sets the fleet status to **New**\. You can track the fleet's status and view other fleet information using these CLI commands: 
+ [describe\-fleet\-events](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-events.html)
+ [describe\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-attributes.html)
+ [describe\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-capacity.html)
+ [describe\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-port-settings.html)
+ [describe\-fleet\-utilization](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-utilization.html)
+ [describe\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-runtime-configuration.html)

You can change the fleet's capacity and other configuration settings as needed using these commands:
+ [update\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-attributes.html)
+ [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html)
+ [update\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html)
+ [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)

------