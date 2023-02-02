# View fleet details<a name="gamelift-console-fleets-metrics"></a>

Access a **Fleet** detail page from the dashboard or the **Fleets** page by choosing the fleet name\.

On the fleet details page you can take the following actions:
+ Update a fleet's attributes, port settings, and runtime configuration\.
+ Add or remove fleet locations\.
+ Change fleet capacity settings\. 
+ Set or change target\-tracking auto\-scaling\.
+ Delete a fleet\.

## Details<a name="fleets-summary"></a>

**Fleet settings**
+ **Fleet ID** – Unique identifier assigned to the fleet\.
+ **Name** – The name of the fleet\.
+ **ARN** – The identifier assigned to this fleet\. A fleet's ARN identifies it as an GameLift resource and specifies the region and AWS account\.
+ **Description** – A short identifiable description of the fleet\.
+ **Status** – Current status of the fleet, which may be **New**, **Downloading**, **Building**, and **Active**\.
+ **Creation time** – The date and time when the fleet was created\.
+ **Termination time** – The date and time the fleet was terminated\. This is blank if the fleet is still active\.
+ **Fleet type** – Indicates whether the fleet uses on\-demand or spot instances\.
+ **EC2 type** – Amazon EC2 [instance type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) selected for the fleet when it was created\.
+ **Instance role** – An AWS IAM role that manages access to your other AWS resources, if one was provided during fleet creation\.
+ **TLS certificate** – Whether the fleet is enabled or disabled to use a TLS certificate for authenticating a game server and encrypting all client/server communication\.
+ **Metric group** – The group used to aggregate metrics for multiple fleets\.
+ **Game scaling protection policy** – Current setting for [game session protection](gamelift-howitworks.md#gamelift-howitworks-capacity) for the fleet\.
+ **Maximum game sessions per player** – The maximum number of sessions a player can create during the **Policy period**\.
+ **Policy period** – How long to wait until resetting the number of sessions a player has created\.

**Build details**  
The **Build details** section displays the build hosted on the fleet\. Select the build name to see the full build detail page\.

**Runtime configuration**  
The **Runtime configuration** section displays the server processes to launch on each instance\. It includes the path for the game server executable and optional launch parameters\.

**Game session activation**  
The **Game session activation** section displays the number of server processes that launch at the same time and how long to wait for the process to activate before terminating it\.

**EC2 port settings**  
The **Ports** section displays the fleet's connection permissions, including IP address and port setting ranges\.

## Metrics<a name="fleets-metrics-tab"></a>

The **Metrics** tab displays a graphical representation of fleet metrics over time\. For more information about using metrics in GameLift, see [Monitor GameLift with Amazon CloudWatch](monitoring-cloudwatch.md)\.

## Events<a name="fleets-events-tab"></a>

The **Events** tab provides a log of all events that have occurred on the fleet, including the event code, message, and time stamp\. See [Event](https://docs.aws.amazon.com/gamelift/latest/apireference/API_Event.html) descriptions in the Amazon GameLift API Reference\. 

## Scaling<a name="fleets-scaling-tab"></a>

The **Scaling** tab contains information about fleet capacity, including the current status and capacity changes over time\. It also provides tools to update capacity limits and manage auto\-scaling\. 

**Scaling capacity**  
View current fleet capacity settings for each fleet location\. For more information about changing limits and capacity, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\.
+ **AWS Location** – Name of a location where fleet instances are deployed\. 
+ **Status** – Hosting status of the fleet location\. Location status must be `ACTIVE` to be able to host games\.
+ **Min size** – The smallest number of instances that must be deployed in the location\.
+ **Desired instances** – The target number of active instances to maintain the location\. When active instances and desired instances aren' the same, a scaling event is started to start or shut down instances as needed until active instances equals desired instances\.
+ **Max size** – The most instances that can be deployed in the location\.
+ **Available** – The service limit on instances minus the number of instances in use\. This value tells you the maximum number of instances that you can add to the location\.

**Auto\-scaling policies**  
This section covers information about auto\-scaling policies that are applied to the fleet\. You can set up or update a target\-based policy\. The fleet's rule\-based policies, which must be defined using the AWS SDK or CLI, are displayed here\. For more information about scaling, see [Auto\-scale fleet capacity with GameLift](fleets-autoscaling.md)\.

**Scaling history**  
View graphs of capacity changes over time\.

## Locations<a name="fleets-location-tab"></a>

The **Locations** tab lists all locations where fleet instances are deployed\. Locations include the fleet's home Region and any remote locations that have been added\. You can add or remove locations directly in this tab\.
+ **Location** – Name of a location where fleet instances are deployed\. 
+ **Status** – Hosting status of the fleet location\. Location status tracks the process of activating the first instances in the location\. Location status must be `ACTIVE` to be able to host games\.
+ **Active instances** – The number of instances with server processes running on the fleet location\.
+ **Active servers** – The number of game server processes able to host game sessions in the fleet location\.
+ **Game sessions** – The number of game sessions active on instances in the fleet location\.
+ **Player sessions** – The number of player sessions, which represent individual players, that are participating in game sessions that are active in the fleet location\.

## Game sessions<a name="fleets-game-sessions-tab"></a>

The **Game sessions** tab lists past and present game sessions hosted on the fleet, including some detail information\. Choose a game session ID to access additional game session information, including player sessions\. For more information about player sessions, see [View data on game and player sessions](gamelift-console-game-player-sessions-metrics.md)\.