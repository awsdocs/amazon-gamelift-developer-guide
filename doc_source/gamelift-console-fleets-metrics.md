# View fleet details<a name="gamelift-console-fleets-metrics"></a>

You can access detailed information on any fleet, including configuration settings, scaling settings, metrics, and game and player data\. Access a **Fleet** detail page from either the dashboard or the **Fleets** page by clicking the fleet name\.

On this page you can also take the following actions:
+ Update a fleet's attributes, port settings, and runtime configuration\. Choose **Actions: Edit fleet**\. 
+ Add or remove fleet locations\. On the **Locations** tab, add locations and deploy instances to them, or remove locations and shut down hosting activity\.
+ Change fleet capacity settings\. On the **Scaling** tab, edit capacity limits and set capacity to a fixed size for fleet locations, turn auto\-scaling activity on/off for each location\.
+ Set or change target\-tracking auto\-scaling\. On the **Scaling** page, enable the target\-tracking policy and set a buffer size\.
+ Shut down a fleet\. Choose **Actions: Terminate fleet**\.

## Fleet summary<a name="fleets-summary"></a>

The summary table includes the following information:
+ **Status** – Current status of the fleet, which may be **New**, **Downloading**, **Building**, and **Active**\. A fleet must be active before it can host game sessions or accept player connections\.
+ **Fleet ID** – Unique identifier assigned to the fleet\.
+ **OS** – Operating system on each instances in the fleet\. A fleet's OS is determined by the build deployed to it\.
+ **Fleet type** – Indicates whether the fleet uses on\-demand or spot instances\.
+ **EC2 type** – Amazon EC2 [instance type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) selected for the fleet when it was created\. A fleet's instance type specifies the computing hardware and capacity used for each instance in the fleet and determines the instance limits for the fleet\. 
+ **Locations** – Number of locations where fleet instances are deployed, including the fleet's home Region and additional remote locations\.
+ **Active instances** – Number of instances in **Active** status, which means they are currently running game sessions or are ready to run game sessions\. 
+ **Active servers** – Number of server processes currently in an **Active** status in the fleet\. The data has a five\-minute delay\.
+ **Game sessions** – Number of active game sessions running on instances in the fleet\. The data has a five\-minute delay\.
+ **Player sessions** – Number of players connected along with the total number of player slots in active game sessions across the fleet\. For example: 25 \(connected players\) of 100 \(possible players\) means the fleet can support 75 additional players\. The data has a five\-minute delay\.
+ **Protection** – Current setting for [game session protection](gamelift-howitworks.md#gamelift-howitworks-capacity) for the fleet\.

  **Uptime** – Total length of time the fleet has been active\.
+ **Date created** – Date and time indicating when the fleet was created\.

## Metrics<a name="fleets-metrics-tab"></a>

The **Metrics** tab displays a graphical representation of fleet metrics over time\. To view metrics using Amazon CloudWatch, see [Monitor GameLift with Amazon CloudWatch ](monitoring-cloudwatch.md)\.

**To display metrics information in the graph**

1. From the list shown to the left of the graph area, click a metric name to add it to the graph display\. Metric names that are gray are currently not being graphed\. The color key identifies which line matches each graphed metric\. Descriptions of individual metrics can be found at [GameLift metrics for fleets](monitoring-cloudwatch.md#gamelift-metrics-fleet)\. The following categories of metrics are available: 
   + **Instance Counts** – These metrics track changes in capacity and utilization at the instance level over time \(also shown on the Scaling tab\)\.
   + **Game** – These metrics show fleet activity and utilization at the game session level over time\.
   + **Server processes** – These metrics track the status and health of server processes across the fleet\. The GameLift service regularly polls each active server process for its health\.
   + **Instance performance** – These metrics reflect performance of the fleet's computing resources\. See detailed descriptions of each metric at [EC2 instance metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)\.

1. Use the following filters, shown above the graph area, to change how metric data is displayed:
   + **Location** – View aggregate metrics for an entire fleet, or view metrics for an individual fleet location\.
   + **Data & Period** – Offers two options for selecting a date range:
     + Use **Relative** to select a period of time relative to the current time, such as **Last hour**, **Last day**, **Last week**\. 
     + Use **Absolute** to specify a period with an arbitrary start and end date/time\.
   + **Granularity** – Select a length of time to aggregate data points\.
   + **Refresh rate** – Select how often you want the graph display to be updated\.
   + **Time zone** – Select which time format to use in the graph display: **UTC** \(universal coordinated time\) or **Browser time** \(local time\)\.
   + **Show points** – Toggle on or off to display discrete data points \(as circles\) or display lines only\.

## Events<a name="fleets-events-tab"></a>

The **Events** tab provides a log of all events that have occurred on the fleet, including the event code, message, and time stamp\. See [Event](https://docs.aws.amazon.com/gamelift/latest/apireference/API_Event.html) descriptions in the Amazon GameLift API Reference\. 

## Scaling<a name="fleets-scaling-tab"></a>

The **Scaling** tab contains information related to fleet capacity, including the current status and a graphical representation of capacity changes over time\. It also provides tools to update capacity limits and manage auto\-scaling\.

**Scaling history**

View a graphical representation of capacity changes over time\. Use the following filters:
+ **Location** – View aggregate metrics for an entire fleet, or view metrics for an individual fleet location\.
+ **Data & Period** – Offers two options for selecting a date range:
  + Use **Relative** to select a period of time relative to the current time, such as **Last hour**, **Last day**, **Last week**\. 
  + Use **Absolute** to specify a period with an arbitrary start and end date/time\.
+ **Granularity** – Select a length of time to aggregate data points\.
+ **Refresh rate** – Select how often you want the graph display to be updated\.
+ **Format** – Select which time format to use in the graph display: **UTC** \(universal coordinated time\) or **Browser time** \(local time\)\.
+ **Show Points** – Toggle on or off to display discrete data points \(as circles\) or display lines only\.

**Scaling limits**

View current fleet capacity settings, broken out by fleet locations\. You can also change limits and desired capacity in this section \(see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\)\.
+ **Location** – Name of a location where fleet instances are deployed\. 
+ **Status** – Hosting status of the fleet location\. Location status must be `ACTIVE` to be able to host games\.
+ **Minimum** – Hard lower limit on the number of instances to that can be deployed in the location\. Fleet capacity cannot drop below this minimum\.
+ **Desired** – The target number of active instances to maintain the location\. When there's a disparity between a fleet location's active instances and desired instances, a scaling event is initiated to either start or shut down instances as needed until active instances equals desired instances\.
+ **Maximum** – Hard upper limit on the number of instances that can be deployed in the location\. Fleet capacity cannot exceed this maximum\.
+ **Available** – The service limit on instances \(for the fleet instance type, home Region, and remote location\) minus the number of instances currently in use\. This value tells you the maximum number of instances that you can add to the location\.

**Auto\-scaling policies**

This section covers information about auto\-scaling policies that are applied to the fleet identifies whether auto\-scaling is enabled or disabled for each fleet location\. You can set up or update a target\-tracking policy\. The fleet's rule\-based policies, which must be defined using the AWS SDK or CLI, are displayed here\. See [Auto\-scale fleet capacity with GameLift](fleets-autoscaling.md)\.

## Game sessions<a name="fleets-game-sessions-tab"></a>

The **Game sessions** tab lists past and present game sessions hosted on the fleet, including some detail information\. Click a game session ID to access additional game session information, including player sessions\. See [View data on game and player sessions](gamelift-console-game-player-sessions-metrics.md) for more details\.

## Capacity allocation<a name="fleets-launch-tab"></a>

The **Capacity allocation** tab displays the runtime configuration for the fleet, which specifies what server processes to launch on each instance\. It includes the path for the game server executable and optional launch parameters\. You can change the fleet's capacity allocation either by editing the fleet in the console or by using the AWS CLI to update the runtime configuration\.

## Ports<a name="fleets-ports-tab"></a>

The **Ports** tab displays the fleet's connection permissions, including IP address and port setting ranges\. You can change connection permissions by either editing the fleet in the console or using the AWS CLI to update the fleet's port settings\.

## ARNs<a name="fleets-arn-tab"></a>

The **Locations** tab lists all locations that are configured for this fleet, including the fleet's home Region and all remote locations\. In addition, you can use controls in this tab to add or remove locations from the fleet\.
+ **Fleet ARN** – The identifier assigned to this fleet\. A fleet's ARN identifies it as an GameLift resource and specifies the region and AWS account, to ensure that it is a unique identifier\. 
+ **Instance Role ARN** – An identifier for an AWS IAM role that manages access to your other AWS resources, if one was provided during fleet creation\. When a role ARN is associated with the fleet, the game servers and other applications that are running on the fleet are able to access those other AWS resources\. Learn more at [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.

## Locations<a name="fleets-location-tab"></a>

The **Locations** tab lists all locations where fleet instances are deployed\. Locations include the fleet's home Region and any remote locations that have been added\. You can add or remove locations directly in this tab \(see [To update fleet locations](fleets-editing.md#fleets-update-locations)\)\.
+ **Location** – Name of a location where fleet instances are deployed\. 
+ **Status** – Hosting status of the fleet location\. Location status tracks the process of activating the first instances in the location\. Location status must be `ACTIVE` to be able to host games\.
+ **Active instances** – The number of instances with server processes running that are deployed to the fleet location\.
+ **Active servers** – The number of game server processes able to host game sessions that are currently running on instances in the fleet location\.
+ **Game sessions** – The number of game sessions currently being hosted on instances in the fleet location\.
+ **Player sessions** – The number of player sessions, which represent individual players, that are participating in game sessions that are currently being hosted in the fleet location\.

## Build<a name="fleets-build-tab"></a>

The **Build** tab displays the fleet's build\-related configuration, which was set when the fleet was created\. Select the build ID to see the full **build** detail page\.

If your build has been deleted or an error has occurred while attempting to retrieve your build, you may see one of the following status messages:
+ **Deleted** – The build for this fleet was deleted\. Your fleet will still run properly despite the build having been deleted\.
+ **Error** – An error occurred while attempting to retrieve build information for the fleet\.