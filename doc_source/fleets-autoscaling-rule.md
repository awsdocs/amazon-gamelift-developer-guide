# Auto\-Scale with Rule\-Based Policies<a name="fleets-autoscaling-rule"></a>

Rule\-based scaling policies provide fine\-grained control when auto\-scaling a fleet's capacity in response to player activity\. For each policy, you can link fleet scaling to one of several available fleet metrics, identify a trigger point, and customize the responding scale\-up or scale\-down event\. Rule\-based policies are particularly useful for supplementing target\-based scaling to handle special circumstances\. 

A rule\-based policy makes the following statement: "If a fleet metric meets or crosses a threshold value for a certain length of time, then change the fleet's capacity by a specified amount\." This topic describes the syntax used to construct a policy statement and provides help with creating and managing your rule\-based policies\.

## Manage Rule\-Based Policies<a name="fleets-autoscaling-policy-setting-cli"></a>

Create, update, or delete rule\-based policies using the AWS SDK or AWS CLI with the [Amazon GameLift Service API](https://docs.aws.amazon.com/gamelift/latest/apireference/)\. You can view all active policies in the Amazon GameLift console\. 

To temporarily disable all scaling policies for a fleet, use the AWS CLI command [stop\-fleet\-actions](https://docs.aws.amazon.com/cli/latest/reference/gamelift/stop-fleet-actions.html)\. 

**To create or update a rule\-based scaling policy \(AWS CLI\):**

1. **Set capacity limits\.** Set either or both limit values using the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command\. For help, see [To Set Capacity Limits \(AWS CLI\)](fleets-capacity-limits.md#fleets-capacity-limits-cli)\.

1. **Create a new policy\.** Open a command\-line window and use the [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/gamelift/put-scaling-policy.html) command with your policy's parameter settings\. To update an existing policy, specify the policy's name and provide a complete version of the updated policy\.

   ```
   --fleet-id <unique fleet identifier>
   --name "<unique policy name>"
   --policy-type <target- or rule-based policy>
   --metric-name <name of metric>
   --comparison-operator <comparison operator>
   --threshold <threshold integer value>
   --evaluation-periods <number of minutes>
   --scaling-adjustment-type <adjustment type>
   --scaling-adjustment <adjustment amount>
   ```

   Example:

   ```
   aws gamelift put-scaling-policy
   --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa
   --name "Scale up when AGS<50"
   --policy-type RuleBased
   --metric-name AvailableGameSessions
   --comparison-operator LessThanThreshold
   --threshold 50
   --evaluation-periods 10
   --scaling-adjustment-type ChangeInCapacity
   --scaling-adjustment 1
   ```

   *Copyable version:*

   ```
   aws gamelift put-scaling-policy --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa --name "Scale up when AGS<50" --policy-type RuleBased --metric-name AvailableGameSessions --comparison-operator LessThanThreshold --threshold 50 --evaluation-periods 10 --scaling-adjustment-type ChangeInCapacity --scaling-adjustment 1
   ```

**To delete a rule\-based scaling policy using the AWS CLI:**
+ Open a command\-line window and use the [delete\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-scaling-policy.html) command with the fleet ID and policy name\.

  Example:

  ```
  aws gamelift delete-scaling-policy
  --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa
  --name "Scale up when AGS<50"
  ```

  *Copyable version:*

  ```
  aws gamelift delete-scaling-policy --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa --name "Scale up when AGS<50"
  ```

## Syntax for Auto\-Scaling Rules<a name="fleets-autoscaling-rule-syntax"></a>

To construct rule\-based scaling policy statement, you must specify six variables:

If *<metric name>* remains *<comparison operator>* *<threshold value>* for *<evaluation period>*, then change fleet capacity using *<adjustment type>* to/by *<adjustment value>*\. 

For example, this policy statement triggers a scale\-up event whenever a fleet's extra capacity \(available hosting resources not currently in use\) is less than what is needed to handle 50 new game sessions: 

If `AvailableGameSessions` remains at `less than 50` for `15 minutes`, then change fleet capacity using `ChangeInCapacity` by `10 instances`\.

**Metric name**  
To trigger a scaling event, link an auto\-scaling policy to one of the following fleet\-specific metrics\. See [Amazon GameLift Metrics for Fleets](monitoring-cloudwatch.md#gamelift-metrics-fleet) for more complete metric descriptions\.  
+ Activating game sessions
+ Active game sessions
+ Available game sessions
+ Percent available game sessions 
+ Active instances
+ Available player sessions
+ Current player sessions 
+ Idle instances
+ Percent idle instances
The following metrics may be used if the fleet is included in a game session queue:  
+ Queue depth \(fleet specific\) – The number of pending game session requests for which this fleet is the best available hosting location\. 
+ Wait time \(fleet specific\) – Fleet\-specific wait time\. The length of time that the oldest pending game session request has been waiting to be fulfilled\. As with queue depth, this metric reflects only game session requests for which this fleet is the best available hosting location\. A fleet's wait time is equal to the oldest current request's time in queue\. 

**Comparison operator**  
This variable tells Amazon GameLift how to compare the metric data to the threshold value\. Valid comparison operators include greater than \(>\), less than \(<\), greater than or equal \(>=\), or less than or equal \(<=\)\.

**Threshold value**  
When the specified metric value meets or crosses the threshold value, it can trigger a scaling event\. Depending on the metric selected, it may indicate an amount of player sessions, game sessions, instances, or game session requests\. This value is always a positive integer\.

**Evaluation period**  
The metric must meet or cross the threshold value for the full length of the evaluation period before triggering a scaling event\. The evaluation period length is consecutive; if the metric retreats from the threshold, the evaluation period starts over again\.

**Adjustment type and value**  
This set of variables works together to specify how Amazon GameLift should adjust the fleet's capacity when a scaling event is triggered\. Choose from three possible types of adjustments:   
+ **Change in capacity** – Increase or decrease the current capacity by a specified number of instances\. Set the adjustment value to the number of instances to add or remove from the fleet\. Positive values add instances, while negative values remove instances\. For example, an value of "\-10" will scale down the fleet by 10 instances, regardless of the fleet's total size\. 
+ **Percent change in capacity** – Increase or decrease the current capacity by a specified percentage\. Set the adjustment value to the percentage you want to increase or decrease the fleet capacity by\. Positive values add instances, while negative values remove instances\. For example, for a fleet with 50 instances, a percentage change of "20" will add ten instances to the fleet\. 
+ **Exact capacity** – Set desired instances to a specific value\. Set the adjustment value to the exact number of instances that you want to maintain in the fleet\.

## Tips for Rule\-Based Auto\-Scaling<a name="fleets-autoscaling-rule-tips"></a>

The following suggestions can help you get the most out of auto\-scaling with rule\-based policies\. 

### Use multiple policies<a name="fleets-autoscaling-policy-tips-multiples"></a>

You can have multiple auto\-scaling policies in force for a fleet at the same time\. The most common scenario is to have a target\-based policy manage most scaling needs and use rule\-based policies to handle edge cases\. However, there are no limits on using multiple policies\.

Multiple policies behave independently\. Keep in mind that there is no way to control the sequence of scaling events\. For example, if you have multiple policies driving scaling up, it is possible that player activity could trigger multiple scaling events simultaneously\. For example, the effects of two scale up policies can easily be compounded if it is possible for player activity to trigger both metrics\. Also watch for policies that trigger each other\. For example, you could create an infinite loop if you create scale up and scale down policies that sets capacity beyond the threshold of each other\. 

### Set maximum and minimum capacity<a name="fleets-autoscaling-policy-tips-maximums"></a>

Each fleet has a maximum and minimum capacity limit\. This feature is particularly important when using auto\-scaling\. Auto\-scaling will never set capacity to a value outside of this range\. By default, newly created fleets have a minimum of 0 and a maximum of 1\. For your auto\-scaling policy to affect capacity as intended, you must increase the maximum value\. 

Fleet capacity is also constrained by limits on the fleet's instance type and on your AWS account\. You cannot set a minimum and maximum that is outside the service and account limits\.

### Track metrics after a change in capacity<a name="fleets-autoscaling-policy-tips-cooldown"></a>

After changing capacity in response to an auto\-scaling policy, Amazon GameLift waits ten minutes before responding to triggers from the same policy\. This wait allows Amazon GameLift time to add the new instances, launch the game servers, connect players, and start collecting data from the new instances\. During this time, Amazon GameLift continues to evaluate the policy against the metric and track the policy's evaluation period, which restarts once a scaling event is triggered\. This means that a scaling policy could trigger another scaling event immediately after the wait time is over\.

There is no wait time between scaling events triggered by different auto\-scaling policies\. 