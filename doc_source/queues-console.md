# View Your Queues<a name="queues-console"></a>

You can view information on all existing game session placement [queues](queues-intro.md)\. Queues shown include only those created in the selected region\. From the **Queues** page, you can create a new queue, delete existing queues, or open a details page for a selected queue\. A **Queue** detail page contains the queue's configuration and metrics data\. You can also edit or delete the queue\.

**To view the **Queues** page**

1. Choose **Queues** from the Amazon GameLift console's menu\.

   The **Queues** page displays the following summary information for each queue:
   + **Queue name** – The name assigned to the queue\. Requests for new game sessions specify a queue by this name\.
   + **Queue timeout** – Maximum length of time, in seconds, that a game session placement request remains in the queue before timing out\.
   + **Destinations in queue** – Number of fleets listed in the queue configuration\. New game sessions can be placed on any fleet in the queue\.

1. To view details for a queue, including metrics, choose the queue name\. For more information on the queue details page, see [View Queue Details](#queues-console-detail)\.

## View Queue Details<a name="queues-console-detail"></a>

You can access detailed information on any queue, including the queue configuration and metrics\. To open a **Queue** detail page, go to the main **Queues** page and choose a queue name\.

The queue detail page displays a summary table and tabs containing additional information\. On this page you can do the following: 
+ Update the queue's configuration, list of destinations and player latency policies\. Choose **Actions**, **Edit queue**\. 
+ Remove a queue\. After a queue is removed, all requests for new game sessions that reference that queue name will fail\. \(Note that deleted queues can be restored by simply creating a queue with the deleted queue's name\.\) Choose **Actions**, **Delete queue**\.

### Summary<a name="queues-console-detail-summary"></a>

The summary table includes the following information:
+ **Queue name** – The name assigned to the queue\. Requests for new game sessions specify a queue by this name\.
+ **Queue timeout** – Maximum length of time, in seconds, that a game session placement request remains in the queue before timing out\.
+ **Destinations in queue** – Number of fleets listed in the queue configuration\. New game sessions can be placed on any fleet in the queue\.

### Destinations<a name="queues-console-detail-destinations"></a>

The **Destinations** tab shows all fleets or aliases listed for the queue\. Fleets are identified by either a fleet ARN or an alias ARN, which specifies the fleet or alias ID and the region\.

When Amazon GameLift searches the destinations for available resources to host a new game session, it searches in the order listed here\. As long as there is capacity on the first destination listed, new game sessions are placed there\. This is the default order for destinations\. You can have individual game session placement requests override the default order by providing player latency data\. This data tells Amazon GameLift to search for an available destination with the lowest average player latency\. 

To add, edit, or delete destinations, choose **Actions**, **Edit queue**\.

### Player Latency Policies<a name="queues-console-detail-policies"></a>

The **Player latency policies** tab shows all policies that have been defined for the queue\. Policies are listed in the order they are enforced during a game session placement effort\.

To add, edit, or delete player latency policies, choose **Actions**, **Edit queue**\.

### Queue Metrics<a name="queues-console-detail-metrics"></a>

The **Metrics** tab shows a graphical representation of queue metrics over time\.

Queue metrics include a range of information describing placement activity across the entire queue, as well as successful placements broken down by region\. The region\-specific data is useful for tracking where your games are being hosted\. With queues that use FleetIQ and prioritize placements to minimize player latency and hosting costs, regional placement metrics can help to detect issues with the overall queue design\. 

Queue metrics are also available in Amazon CloudWatch\. You can view the descriptions of all metrics at [Amazon GameLift Metrics for Queues](monitoring-cloudwatch.md#gamelift-metrics-queue)\.

**To display metrics information in the graph**

1. Click one or more of the metric names that are listed to the left of the graph area\. Metric names shown in black are displayed in the graph, while those shown in gray are turned off\. Use the color key to identify which graphed line matches a selected metric\. 

1. Use the following filters, shown above the graph area, to change how metric data is displayed:
   + **Data** and **Period** – Offers two options for selecting a date range:
     + Use **Relative** to select a period of time relative to the current time, such as **Last hour**, **Last day**, **Last week**\. 
     + Use **Absolute** to specify a period with an arbitrary start and end date/time\.
   + **Granularity** – Select a length of time to aggregate data points\.
   + **Refresh rate** – Select how often you want the graph display to be updated\. You can refresh the graph any time by clicking the refresh button in the graph's upper right corner\.
   + **Time zone** – Select which time format to use in the graph display: **UTC** \(universal coordinated time\) or **Browser time** \(local time\)\.
   + **Show points** – Toggle to display discrete data points as circles or display lines only\.