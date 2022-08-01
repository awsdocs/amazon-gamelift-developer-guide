# View your queues<a name="queues-console"></a>

You can view information on all existing game session placement queues\. The queues page shows queues created in your current Region\. From the **Queues** page, you can create a new queue, delete existing queues, or open a details page for a selected queue\. Each queue details page contains the queue's configuration and metrics data\. For more information about queues, see [Setting up GameLift queues for game session placement](queues-intro.md)\.

The queues page displays the following summary information for each queue:
+ **Queue name** – The name assigned to the queue\. Requests for new game sessions specify a queue by this name\.
+ **Queue timeout** – Maximum length of time, in seconds, that a game session placement request remains in the queue before timing out\.
+ **Destinations in queue** – Number of fleets listed in the queue configuration\. GameLift places new game sessions on any fleet in the queue\.

## View queue details<a name="queues-console-detail"></a>

You can access detailed information on any queue, including the queue configuration and metrics\. To open a queue details page, go to the **Queues** page and choose a queue name\.

The queue detail page displays a summary table and tabs containing additional information\. On this page you can do the following: 
+ Update the queue's configuration, list of destinations and player latency policies\. Choose **Edit**\. 
+ Delete a queue\. After you delete a queue, all requests for new game sessions that reference that queue name will fail\. Choose **Delete**\.
**Note**  
To restore a deleted queue, create a new queue with the deleted queue's name\.

### Details<a name="queues-console-detail-summary"></a>

**Overview**  
The **Overview** section displays the queue's Amazon Resource Name \(**ARN**\) and the **Timeout**\. You can use the ARN when referencing the queue in other actions or areas of GameLift\. The timeout is the maxiumum length of time, in seconds, that a game session placement request remains in the queue before timing out\.

**Event notification**  
The **Event notification** section lists the **SNS topic** GameLift publishes event notifications to and the **Event data** that is added to all events created by this queue\.

**Tags**  
The **Tags** table displays the keys and values used to tag the resource\. For more information about tagging, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

### Metrics<a name="queues-console-detail-metrics"></a>

The **Metrics** tab shows a graphical representation of queue metrics over time\.

Queue metrics include a range of information describing placement activity across the queue, including successful placements organized by Region\. You can use Region data to understand where you are hosting your games\. Regional placement metrics can help to detect issues with the overall queue design\. 

Queue metrics are also available in Amazon CloudWatch\. For descriptions of available metrics, see [GameLift metrics for queues](monitoring-cloudwatch.md#gamelift-metrics-queue)\.

### Destinations<a name="queues-console-detail-destinations"></a>

The **Destinations** tab shows all fleets or aliases listed for the queue\.

When Amazon GameLift searches the destinations for available resources to host a new game session, it searches the default order listed here\. As long as there is capacity on the first destination listed, GameLift places new game sessions there\. You can have individual game session placement requests override the default order by providing player latency data\. This data tells Amazon GameLift to search for an available destination with the lowest average player latency\. For more information about designing your queues, see [Design a game session queue](queues-design.md)\.

### Session placement<a name="queues-console-detail-policies"></a>

**Player latency policies**  
The **Player latency policies** section shows all policies that the queue uses\. The tables lists the policies in the order they're enforced\.

**Locations**  
The **Locations** section shows the locations that this queue can put a game session in\.

**Priority**  
The **Priority** section shows the order that the queue evaluates a game sessions details\.

**Location order**  
The **Location order** section shows the default order that the queue uses when placing game sessions\. The queue uses this order if you haven't defined other types of priority\.