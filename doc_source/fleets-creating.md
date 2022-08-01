# Deploy a fleet<a name="fleets-creating"></a>

Use either the [GameLift console](https://console.aws.amazon.com/gamelift/) or the AWS Command Line Interface \(AWS CLI\) to create a fleet\. 

After you create a new fleet, the fleet's status passes through several stages as GameLift deploys the fleet and installs and starts the game servers\. The fleet is ready to host game sessions, after it reaches `ACTIVE` status\. For help with fleet creation issues, see [Debug GameLift fleet issues](fleets-creating-debug.md)\.<a name="fleets-creating-builds-fleets-page"></a><a name="fleets-creating-aws-cli"></a>

------
#### [ Console ]

1. In the [GameLift Console](https://console.aws.amazon.com/gamelift/), in the navigation pane, choose **Fleets**\.

1. On the **Fleets** page, choose **Create fleet**\.

1. On the **Create fleet** page, under **Fleet details** do the following:

   1. For **Name**, enter a fleet name\. We recommend including the fleet type \(Spot or On\-demand\) in your fleet names\. This makes it much easier to identify fleet types when viewing a list of fleets\.

   1. For **Description**, provide a short description of the fleet\.

   1. For **Binary type**, select **Build** or **Script** to define the game server type that GameLift deploys to this fleet\.

   1. Select a **Script** or **Build** from the dropdown list of uploaded scripts or builds\.

1. \(Optional\) Under **Additional details** for the following:

   1. For **Instance role**, choose an IAM role to associate with this fleet\. This allows the instances in the fleet to assume the chosen role, which provide access to the AWS services defined in the role\. For more information about fleets working with AWS services, see [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\. To create a fleet with an instance role, your account must have the IAM `PassRole` permission\. For more information, see [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\.

      You can't update the instance role after fleet creation\.

   1. For **Certification generation**, choose to have GameLift **Generate a TLS certificate** for the fleet\. You can use a fleet TLS certificate to have your game client authenticate a game server when connecting, and encrypt all client/server communication\. For each instance in a TLS\-enabled fleet, GameLift also creates a new DNS entry with the certificate\. Use these resources to set up authentication and encryption for your game\.

   1. For **Metric group**, Enter the name of a new or existing fleet metric group\. You can aggregate the metrics for multiple fleets by adding them to the same metric group\.

      You can't update the metric group after fleet creation\.

1. Choose **Next**\.

1. On the **Select locations** page, select one or more additional remote locations to deploy instances to\. The home Region is automatically selected based on the Region you are accessing the console from\. If you select additional locations, fleet instances are also deployed in these locations\. 
**Important**  
To use opt\-in Regions, enable them in your AWS account\.  
Fleets with instances in opt\-in Regions created before February 28, 2022 are unaffected\.
To create new multi\-location fleets or update existing multi\-location fleets, first enable any opt\-in Regions that you choose to use\.
For more information about opt\-in Regions and how to enable them, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\.

1. Choose **Next**\.

1. On the **Define instance details page**, choose **On\-demand** or **Spot** instances for this fleet\. For more information about fleet types, see [Choosing computing resources](gamelift-ec2-instances.md)\.

1. Select an Amazon EC2 **Instance type** from the list\. For more information about choosing an instance type, see [Choosing computing resources](gamelift-ec2-instances.md)\. After you create the fleet, you can't change the instance type\.

1. Choose **Next**\.

1. On the **Configure runtime** page, under **Runtime configuration** do the following:

   1. For **Launch path**, enter the path to the game executable in your build or script\. On Windows instances, game servers are built to the path `C:\game`\. On Linux instances, game servers are built to `/local/game`\. Examples: **C:\\game\\MyGame\\server\.exe**, **/local/game/MyGame/server\.exe**, or **MyRealtimeLaunchScript\.js**\.

   1. \(Optional\) For **Launch parameters**, enter information to pass to your game executable as a set of command line parameters\. Example: **\+sv\_port 33435 \+start\_lobby**\.

   1. For **Concurrent processes**, choose the number of server processes to run concurrently on each instance in the fleet\. Review the GameLift [limits](https://aws.amazon.com//ec2/faqs/#Is_Amazon_EC2_used_in_conjunction_with_Amazon_S3) on number of concurrent server processes\. 

      Limits on concurrent server processes per instance apply to the total of concurrent processes for all configurations\. If you configure the fleet to exceed the limit, the fleet can't activate\.

1. Under **Game session activation**, provide limits for activating new game sessions on the instances in this fleet:

   1. For **Max concurrent game session activation**, enter the number of game sessions on an instance that activate at the same time\. This limit is useful when launching multiple new game sessions may have an impact on the performance of other game sessions running on the instance\.

   1. For **New activation timeout**, enter how long to wait for a session to activate\. If the game session doesn't move to `ACTIVE` status before the timeout, GameLift terminates the game session activation\. 

1. \(Optional\) Under **EC2 port settings**, do the following:

   1. Choose **Add port setting** to define access permissions for inbound traffic connecting to the server process deployed on the fleet\.

   1. For **Type**, choose **Custom TCP** or **Custom UDP**\.

   1. For **Port range**, Enter a range of port numbers that allow inbound connections\. A port range must use the format `nnnnn[-nnnnn]`, with values between 1025 and 60000\. Example: **1500** or **1500\-20000**\. 

   1. For **IP address range**, Enter a range of IP addresses\. Use CIDR notation\. Example: **0\.0\.0\.0/0** \(This example allows access to anyone trying to connect\.\)

1. \(Optional\) Under **Game session resource settings** do the following:

   1. For **Game scaling protection policy**, Turn on or off scaling protection\. GameLift won't terminate instance with protection during a scale down event if they're hosting an active game session\.

   1. For **Resource creation limit**, enter a maximum number of game sessions a player can create during the policy period\.

1. Choose **Next**\.

1. \(Optional\) Add tags to the build by entering **Key** and **Value** pairs\. Choose **Next** to continue to fleet creation review\.

1. Choose **Create**\. GameLift assigns an ID to the new fleet and begins the fleet activation process\. You can track the new fleet's status on the **Fleets** page\. 

You can update the fleet's metadata and configuration at any time, regardless of fleet status\. For more information, see [Manage your GameLift fleets](fleets-editing.md)\. You can update fleet capacity after the fleet has reached ACTIVE status\. For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\. You can also add or remove remote locations\.

------
#### [ AWS CLI ]

To create a fleet with the AWS CLI, open a command line window and use the `create-fleet` command\. For more information about the `create-fleet` command, see [https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet.html) in the *AWS CLI Command Reference*\.

The example `create-fleet` request shown below creates a new fleet with the following characteristics: 
+ The fleet uses c5\.large On\-Demand Instances with the operating system that's appropriate for the selected game build\. 
+ It deploys the specified game server build, which must be in a **Ready** status to the following locations: 
  + us\-west\-2 \(home Region\)
  + sa\-east\-1 \(remote location\)
+ TLS certificate generation is enabled\.
+ Each instance in the fleet will run ten identical processes of the game server concurrently, enabling each instance to host up to ten game sessions simultaneously\.
+ On each instance, GameLift allows two new game sessions to activate at the same time\. It also terminates any activating game session if they aren't ready to host players within 300 seconds\.
+ All game sessions hosted on instances in this fleet have game session protection turned on\.
+ Individual players can create three new game sessions within a 15\-minute period\.
+ Each game session hosted on this fleet has a connection point that falls within the specified IP address and port ranges\.
+ GameLift adds metrics for this fleet to the `EMEAfleets` metric group, which \(in this example\) combines metrics for all fleets in EMEA Regions\. 

```
aws gamelift create-fleet \
    --name SampleFleet123 \
    --description "The sample test fleet" \
    --ec2-instance-type c5.large \
    --region us-west-2 \
    --locations "Location=sa-east-1" \
    --fleet-type ON_DEMAND \
    --build-id build-92f061ed-27c9-4a02-b1f4-6f85b2385620 \
    --certificate-configuration "CertificateType=GENERATED" \
    --runtime-configuration "GameSessionActivationTimeoutSeconds=300, MaxConcurrentGameSessionActivations=2, ServerProcesses=[{LaunchPath=C:\game\Bin64.dedicated\MultiplayerSampleProjectLauncher_Server.exe, Parameters=+sv_port 33435 +start_lobby, ConcurrentExecutions=10}]" \
    --new-game-session-protection-policy "FullProtection" \
    --resource-creation-limit-policy "NewGameSessionsPerCreator=3, PolicyPeriodInMinutes=15" \
    --ec2-inbound-permissions "FromPort=33435,ToPort=33435,IpRange=0.0.0.0/0,Protocol=UDP" "FromPort=33235,ToPort=33235,IpRange=0.0.0.0/0,Protocol=UDP" \
    --metric-groups  "EMEAfleets"
```

If the create\-fleet request is successful, GameLift returns a set of fleet attributes that includes the configuration settings you requested and a new fleet ID\. GameLift then initiates the fleet activation process and sets the fleet status and the location statuses to **New**\. You can track the fleet's status and view other fleet information using these CLI commands: 
+ [describe\-fleet\-events](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-events.html)
+ [describe\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-attributes.html)
+ [describe\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-capacity.html)
+ [describe\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-port-settings.html)
+ [describe\-fleet\-utilization](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-utilization.html)
+ [describe\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-runtime-configuration.html)
+ [describe\-fleet\-location\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-location-attributes.html)
+ [describe\-fleet\-location\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-location-capacity.html)
+ [describe\-fleet\-location\-utilization](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-location-utilization.html)

You can change the fleet's capacity and other configuration settings as needed using these commands:
+ [update\-fleet\-attributes](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-attributes.html)
+ [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html)
+ [update\-fleet\-port\-settings](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html)
+ [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)
+ [create\-fleet\-locations](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-fleet-locations.html)
+ [delete\-fleet\-locations](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-fleet-locations.html)

------