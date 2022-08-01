# Monitor GameLift with Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

You can monitor GameLift using Amazon CloudWatch, an AWS service that collects raw data and processes it into readable, near real\-time metrics\. These statistics are kept for 15 months to provide a historical perspective on how your game server hosting with GameLift is performing\. You can set alarms that watch for certain thresholds and send notifications or take actions when those thresholds are met\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

The following tables list the metrics and dimensions for GameLift\. All metrics that are available in CloudWatch are also available in the GameLift console, which provides the data as a set of customizable graphs\. To access CloudWatch metrics for your games, use the AWS Management Console, the AWS CLI, or the CloudWatch API\. 

If a metric does not have a location, it uses the home location\.

## Dimensions for GameLift metrics<a name="billing-metricdimensions-fleet"></a>

GameLift supports filtering metrics by the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| `Location` | Filter metrics for a fleet deployment location\. If a metric does not have a location, it uses the home location\. | 
|  `FleetId`  |  Filter metrics for a single fleet\. This dimension can be used with all fleet metrics for instances, server processes, game sessions, and player sessions\.   | 
|  `MetricGroup`  |  Filter metrics for a collection of fleets\. Add a fleet to a metric group by adding the metric group name to the fleet's attributes \(see [UpdateFleetAttributes\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateFleetAttributes.html)\)\. This dimension can be used with all fleet metrics for instances, server processes, game sessions, and player sessions\.  | 
|  `QueueName`  |  Filter metrics for a single queue\. This dimension is used with metrics for game session queues only\.   | 
|  `ConfigurationName`  |  Filter metrics for a single matchmaking configuration\. This dimension is used with metrics for matchmaking configurations\.   | 
|  `ConfigurationName-RuleName`  |  Filter metrics for an intersect of a matchmaking configuration and matchmaking rule\. This dimension is used with metrics for matchmaking rules only\.   | 
|  `InstanceType`  |  Filter metrics for an EC2 instance type designation, such as "c4\.large"\. This dimension is used with metrics for spot instances\.   | 
|  `OperatingSystem`  |  Filter metrics for an instance's operating system This dimension is used with metrics for spot instances\.   | 
| `GameServerGroup` | Filter FleetIQ metrics for a game server group\. | 

## GameLift metrics for fleets<a name="gamelift-metrics-fleet"></a>

The `AWS/GameLift` namespace includes the following metrics related to activity across a fleet or a group of fleets\. Fleets are used with a managed GameLift solution\. The GameLift service sends metrics to CloudWatch every minute\. 

### Instances<a name="gamelift-metrics-fleet-instances"></a>


| Metric | Description | 
| --- | --- | 
| `ActiveInstances` |  Instances with ACTIVE status, which means they are running active server processes\. The count includes idle instances and those that are hosting one or more game sessions\. This metric measures current total instance capacity\. This metric can be used with automatic scaling\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `DesiredInstances` |  Target number of active instances that GameLift is working to maintain in the fleet\. With automatic scaling, this value is determined based on the scaling policies currently in force\. Without automatic scaling, this value is set manually\. This metric is not available when viewing data for fleet metric groups\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `IdleInstances` |  Active instances that are currently hosting zero \(0\) game sessions\. This metric measures capacity that is available but unused\. This metric can be used with automatic scaling\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `MaxInstances` |  Maximum number of instances that are allowed for the fleet\. A fleet's instance maximum determines the capacity ceiling during manual or automatic scaling up\. This metric is not available when viewing data for fleet metric groups\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `MinInstances` |  Minimum number of instances allowed for the fleet\. A fleet's instance minimum determines the capacity floor during manual or automatic scaling down\. This metric is not available when viewing data for fleet metric groups\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `PercentIdleInstances` |  Percentage of all active instances that are idle \(calculated as `IdleInstances / ActiveInstances`\)\. This metric can be used for automatic scaling\. Units: Percent Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `RecycledInstances` |  Number of spot instances that have been recycled and replaced\. GameLift recycles spot instances that are not currently hosting game sessions and have a high probability of interruption\.  Units: Count Relevant CloudWatch statistics: Sum, Average, Minimum, Maximum Dimensions: Location | 
| `InstanceInterruptions` |  Number of spot instances that have been interrupted\. Units: Count Relevant CloudWatch statistics: Sum, Average, Minimum, Maximum Dimensions: Location | 

### Server processes<a name="gamelift-metrics-fleet-processes"></a>


| Metric | Description | 
| --- | --- | 
| `ActiveServerProcesses` |  Server processes with ACTIVE status, which means they are running and able to host game sessions\. The count includes idle server processes and those that are hosting game sessions\. This metric measures current total server process capacity\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `HealthyServerProcesses` |  Active server processes that are reporting healthy\. This metric is useful for tracking the overall health of the fleet's game servers\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
|  `PercentHealthyServerProcesses`  |  Percentage of all active server processes that are reporting healthy \(calculated as `HealthyServerProcesses / ActiveServerProcesses`\)\. Units: Percent Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location  | 
| `ServerProcessAbnormalTerminations` |  Server processes that were shut down due to abnormal circumstances since the last report\. This metric includes terminations that were initiated by the GameLift service\. This occurs when a server process stops responding, consistently reports failed health checks, or does not terminate cleanly \(by calling [ ProcessEnding\(\)](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-server-sdk-cpp-ref-actions.html#integration-server-sdk-cpp-ref-processending)\)\. Units: Count Relevant CloudWatch statistics: Sum, Average, Minimum, Maximum Dimensions: Location | 
| `ServerProcessActivations` |  Server processes that successfully transitioned from ACTIVATING to ACTIVE status since the last report\. Server processes cannot host game sessions until they are active\. Units: Count Relevant CloudWatch statistics: Sum, Average, Minimum, Maximum Dimensions: Location | 
| `ServerProcessTerminations` |  Server processes that were shut down since the last report\. This includes all server processes that transitioned to TERMINATED status for any reason, including normal and abnormal process terminations\. Units: Count Relevant CloudWatch statistics: Sum, Average, Minimum, Maximum Dimensions: Location | 

### Game sessions<a name="gamelift-metrics-fleet-games"></a>


| Metric | Description | 
| --- | --- | 
| `ActivatingGameSessions` |  Game sessions with ACTIVATING status, which means they are in the process of starting up\. Game sessions cannot host players until they are active\. High numbers for a sustained period of time may indicate that game sessions are not transitioning from ACTIVATING to ACTIVE status\. This metric can be used with automatic scaling\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `ActiveGameSessions` |  Game sessions with ACTIVE status, which means they are able to host players, and are hosting zero or more players\. This metric measures the total number of game sessions currently being hosted\. This metric can be used with automatic scaling\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `AvailableGameSessions` |  Active, healthy server processes that are not currently being used to host a game session and can start a new game session without a delay to spin up new server processes or instances\. This metric can be used with automatic scaling\.  For fleets that limit concurrent game session activations, use the metric `ConcurrentActivatableGameSessions`\. That metric more accurately represents the number of new game sessions that can start without any type of delay\.   Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `ConcurrentActivatableGameSessions` |  Active, healthy server processes that are not currently being used to host a game session and can immediately start a new game session\. This metric differs from `AvailableGameSessions` in the following way: it does not count server processes that currently cannot activate a new game session because of limits on game session activations\. \(See the fleet [RuntimeConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RuntimeConfiguration.html) optional setting `MaxConcurrentGameSessionActivations`\)\. For fleets that don't limit game session activations, this metric is identical to `AvailableGameSessions`\.  Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: Location | 
| `PercentAvailableGameSessions` |  Percentage of game session slots on all active server processes \(healthy or unhealthy\) that are not currently being used \(calculated as `AvailableGameSessions / [ActiveGameSessions + AvailableGameSessions + unhealthy server processes]`\)\. This metric can be used with automatic scaling\. Units: Percent Relevant CloudWatch statistics: Average Dimensions: Location | 
| `GameSessionInterruptions` |  Number of game sessions on spot instances that have been interrupted\. Units: Count Relevant CloudWatch statistics: Sum, Average, Minimum, Maximum Dimensions: Location | 

### Player sessions<a name="gamelift-metrics-fleet-players"></a>


| Metric | Description | 
| --- | --- | 
| `CurrentPlayerSessions` |  Player sessions with either ACTIVE status \(player is connected to an active game session\) or RESERVED status \(player has been given a slot in a game session but hasn't yet connected\)\. This metric can be used with automatic scaling\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum | 
| `PlayerSessionActivations` |  Player sessions that transitioned from RESERVED status to ACTIVE since the last report\. This occurs when a player successfully connects to an active game session\. Units: Count Relevant CloudWatch statistics: Sum, Average, Minimum, Maximum | 

## GameLift metrics for queues<a name="gamelift-metrics-queue"></a>

The `GameLift` namespace includes the following metrics related to activity across a game session placement queue\. Queues are used with a managed GameLift solution\. The GameLift service sends metrics to CloudWatch every minute\.


| Metric | Description | 
| --- | --- | 
| `AverageWaitTime` | Average amount of time that game session placement requests in the queue with status PENDING have been waiting to be fulfilled\. Units: Seconds Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum Dimensions: Location  | 
|  `FirstChoiceNotViable`  |  Game sessions that were successfully placed but NOT in the first\-choice fleet, because that fleet was considered not viable \(such as a spot fleet with a high interruption rate\)\. This metric is based on cost, not latency\. The first\-choice fleet is either the first fleet listed in the queue or—when a placement request includes player latency data—it is the first fleet chosen by [FleetIQ prioritization](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-design.html#queues-design-fleetiq)\. If there are no viable spot fleets, any fleet in that region may be selected\.  Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum  | 
|  `FirstChoiceOutOfCapacity`  |  Game sessions that were successfully placed but NOT in the first\-choice fleet, because that fleet had no available resources\. The first\-choice fleet is either the first fleet listed in the queue or—when a placement request includes player latency data —it is the first fleet chosen by [FleetIQ prioritization](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-design.html#queues-design-fleetiq)\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum  | 
|  `LowestLatencyPlacement`  |  Game sessions that were successfully placed in a region that offers the queue's lowest possible latency for the players\. This metric is emitted only when player latency data is included in the placement request, which triggers [FleetIQ prioritization](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-design.html#queues-design-fleetiq)\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum  | 
|  `LowestPricePlacement`  | Game sessions that were successfully placed in a fleet with the queue's lowest possible price for the chosen region\. \([FleetIQ prioritization](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-design.html#queues-design-fleetiq) first chooses the region with the lowest latency for the players and then finds the lowest cost fleet within that region\.\) This fleet can be either a spot fleet or an on\-demand instance if the queue has no spot instances\. This metric is emitted only when player latency data is included in the placement request\. Units: CountRelevant CloudWatch statistics: Average, Minimum, Maximum, Sum | 
|  `Placement <region name>`  | Game sessions that are successfully placed in fleets located in the specified region\. This metric breaks down the `PlacementsSucceeded` metric by region\.Units: CountRelevant CloudWatch statistics: Sum | 
| `PlacementsCanceled` | Game session placement requests that were canceled before timing out since the last report\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum | 
|  `PlacementsFailed`  |  Game session placement requests that failed for any reason since the last report\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum  | 
| `PlacementsStarted` |  New game session placement requests that were added to the queue since the last report\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum | 
| `PlacementsSucceeded` | Game session placement requests that resulted in a new game session since the last report\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum | 
| `PlacementsTimedOut` |  Game session placement requests that reached the queue's timeout limit without being fulfilled since the last report\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum | 
| `QueueDepth` | Number of game session placement requests in the queue with status PENDING\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum Dimensions: Location  | 

## GameLift metrics for matchmaking<a name="gamelift-metrics-match"></a>

The `GameLift` namespace includes metrics on FlexMatch activity for matchmaking configurations and matchmaking rules\. FlexMatch matchmaking is used with a managed GameLift solution\. The GameLift service sends metrics to CloudWatch every minute\.

For more information on the sequence of matchmaking activity, see [How Amazon GameLift FlexMatch works](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/gamelift-match.html)\.

### Matchmaking configurations<a name="gamelift-metrics-match-configs"></a>


| Metric | Description | 
| --- | --- | 
| `CurrentTickets` | Matchmaking requests currently being processed or waiting to be processed\. Units: Count Relevant CloudWatch statistics: Average, Minimum, Maximum, Sum | 
| `MatchAcceptancesTimedOut` | For matchmaking configurations that require acceptance, the potential matches that timed out during acceptance since the last report\. Units: Count Relevant CloudWatch statistics: Sum | 
| `MatchesAccepted` | For matchmaking configurations that require acceptance, the potential matches that were accepted since the last report\. Units: Count Relevant CloudWatch statistics: Sum | 
| `MatchesCreated` | Potential matches that were created since the last report\. Units: Count Relevant CloudWatch statistics: Sum | 
| `MatchesPlaced` | Matches that were successfully placed into a game session since the last report\. Units: Count Relevant CloudWatch statistics: Sum | 
| `MatchesRejected` | For matchmaking configurations that require acceptance, the potential matches that were rejected by at least one player since the last report\. Units: Count Relevant CloudWatch statistics: Sum | 
| `PlayersStarted` | Players in matchmaking tickets that were added since the last report\. Units: Count Relevant CloudWatch statistics: Sum | 
| `TicketsFailed` |  Matchmaking requests that resulted in failure since the last report\.  Units: Count Relevant CloudWatch statistics: Sum  | 
| `TicketsStarted` |  New matchmaking requests that were created since the last report\. Units: Count Relevant CloudWatch statistics: Sum  | 
| `TicketsTimedOut` | Matchmaking requests that reached the timeout limit since the last report\. Units: Count Relevant CloudWatch statistics: Sum | 
| `TimeToMatch` | For matchmaking requests that were put into a potential match before the last report, the amount of time between ticket creation and potential match creation\. Units: Seconds Relevant CloudWatch statistics: Data Samples, Average, Minimum, Maximum | 
| `TimeToTicketCancel` |  For matchmaking requests that were canceled before the last report, the amount of time between ticket creation and cancellation\. Units: Seconds Relevant CloudWatch statistics: Data Samples, Average, Minimum, Maximum | 
| `TimeToTicketSuccess` | For matchmaking requests that succeeded before the last report, the amount of time between ticket creation and successful match placement\. Units: Seconds Relevant CloudWatch statistics: Data Samples, Average, Minimum, Maximum | 

### Matchmaking rules<a name="gamelift-metrics-match-rules"></a>


| Metric | Description | 
| --- | --- | 
| `RuleEvaluationsPassed` | Rule evaluations during the matchmaking process that passed since the last report\. This metric is limited to the top 50 rules\.Units: CountRelevant CloudWatch statistics: Sum | 
| `RuleEvaluationsFailed` | Rule evaluations during matchmaking that failed since the last report\. This metric is limited to the top 50 rules\. Units: Count Relevant CloudWatch statistics: Sum | 

## GameLift metrics for FleetIQ<a name="gamelift-metrics-fiq"></a>

The `GameLift` namespace includes metrics for FleetIQ game server group and game server activity as part of a FleetIQ standalone solution for game hosting\. The GameLift service sends metrics to CloudWatch every minute\. Also see [Monitoring your Auto Scaling groups and instances using amazon CloudWatch](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-monitoring.html) in the *Amazon EC2 Auto Scaling User Guide*\.


| Metric | Description | 
| --- | --- | 
| `AvailableGameServers` |  Game servers that are available to run a game execution and are not currently occupied with gameplay\. This number includes game servers that have been claimed but are still in AVAILABLE status\. Units: Count Relevant CloudWatch statistics: Sum Dimensions: GameServerGroup | 
| `UtilizedGameServers` | Game servers that are currently occupied with gameplay\. This number includes game servers that are in UTILIZED status\. Units: Count Relevant CloudWatch statistics: Sum Dimensions: GameServerGroup | 
|  `DrainingAvailableGameServers`  | Game servers on instances scheduled for termination that are currently not supporting gameplay\. These game servers are the lowest priority to be claimed in response to a new claim request\. Units: Count Relevant CloudWatch statistics: Sum Dimensions: GameServerGroup | 
|  `DrainingUtilizedGameServers`  | Game servers on instances scheduled for termination that are currently supporting gameplay\. Units: Count Relevant CloudWatch statistics: Sum Dimensions: GameServerGroup | 
|  `PercentUtilizedGameServers`  |  Portion of game servers that are currently supporting game executions\. This metric indicates the amount of game server capacity that is currently in use\. It is useful for driving an Auto Scaling policy that can dynamically add and remove instances to match with player demand\. Units: Percent Relevant CloudWatch statistics: Average, Minimum, Maximum Dimensions: GameServerGroup  | 
|  `GameServerInterruptions`  | Game servers on Spot Instances that were interrupted due to limited Spot availability\. Units: Count Relevant CloudWatch statistics: Sum Dimensions: GameServerGroup, InstanceType | 
|  `InstanceInterruptions`  | Spot Instances that were interrupted due to limited availability\. Units: Count Relevant CloudWatch statistics: Sum Dimensions: GameServerGroup, InstanceType | 