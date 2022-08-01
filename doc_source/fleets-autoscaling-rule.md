# Auto scale with rule\-based policies<a name="fleets-autoscaling-rule"></a>

Rule\-based scaling policies in GameLift provide fine\-grained control when auto scaling a fleet's capacity in response to player activity\. For each policy, you can link scaling to one of several fleet metrics, identify a trigger point, and customize the responding scale\-up or scale\-down event\. Rule\-based policies are useful for supplementing [target\-based scaling](fleets-autoscaling-target.md) to handle special circumstances\. 

A rule\-based policy states the following: "If a fleet metric meets or crosses a threshold value for a certain length of time, then change the fleet's capacity by a specified amount\." This topic describes the syntax used to construct a policy statement and provides help with creating and managing your rule\-based policies\.

## Manage rule\-based policies<a name="fleets-autoscaling-policy-setting-cli"></a>

Create, update, or delete rule\-based policies using an AWS SDK or the AWS Command Line Interface \(AWS CLI\) with the [GameLift service API](https://docs.aws.amazon.com/gamelift/latest/apireference/Welcome.html)\. You can view all active policies in the GameLift console\.

To temporarily stop all scaling policies for a fleet, use the AWS CLI command [stop\-fleet\-actions](https://docs.aws.amazon.com/cli/latest/reference/gamelift/stop-fleet-actions.html)\.

**To create or update a rule\-based scaling policy \(AWS CLI\):**

1. **Set capacity limits\.** Set either or both limit values using the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command\. For more information, see [Set GameLift capacity limits](fleets-capacity-limits.md)\.

1. **Create a new policy\.** Open a command line window and use the [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/gamelift/put-scaling-policy.html) command with your policy's parameter settings\. To update an existing policy, specify the policy's name and provide a complete version of the updated policy\.

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
   aws gamelift put-scaling-policy \
       --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa \
       --name "Scale up when AGS<50" \
       --policy-type RuleBased \
       --metric-name AvailableGameSessions \
       --comparison-operator LessThanThreshold \
       --threshold 50 \
       --evaluation-periods 10 \
       --scaling-adjustment-type ChangeInCapacity \
       --scaling-adjustment 1
   ```

**To delete a rule\-based scaling policy using the AWS CLI:**
+ Open a command line window and use the [delete\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-scaling-policy.html) command with the fleet ID and policy name\.

  Example:

  ```
  aws gamelift delete-scaling-policy \
      --fleet-id fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa \
      --name "Scale up when AGS<50"
  ```

## Syntax for auto scaling rules<a name="fleets-autoscaling-rule-syntax"></a>

To construct a rule\-based scaling policy statement, specify six variables:

If *<metric name>* remains *<comparison operator>* *<threshold value>* for *<evaluation period>*, then change fleet capacity using *<adjustment type>* to/by *<adjustment value>*\.

For example, this policy statement starts a scale\-up event whenever a fleet's extra capacity is less than what's needed to handle 50 new game sessions:

If `AvailableGameSessions` remains at `less than 50` for `10 minutes`, then change fleet capacity using `ChangeInCapacity` by `1 instances`\.

**Metric name**  
To start a scaling event, link an auto scaling policy to one of the following fleet\-specific metrics\. For complete metric descriptions, see [GameLift metrics for fleets](monitoring-cloudwatch.md#gamelift-metrics-fleet)\.  
+ Activating game sessions
+ Active game sessions
+ Available game sessions
+ Percent available game sessions
+ Active instances
+ Available player sessions
+ Current player sessions
+ Idle instances
+ Percent idle instances
If the fleet is in a game session queue, you can use the following metrics:  
+ Queue depth – The number of pending game session requests this fleet is the best available hosting location for\.
+ Wait time – Fleet\-specific wait time\. The length of time that the oldest pending game session request has been waiting to be fulfilled\. A fleet's wait time is equal to the oldest current request's time in queue\.

**Comparison operator**  
Tells GameLift how to compare the metric data to the threshold value\. Valid comparison operators include greater than \(>\), less than \(<\), greater than or equal \(>=\), and less than or equal \(<=\)\.

**Threshold value**  
When the specified metric value meets or crosses the threshold value, it starts a scaling event\. This value is always a positive integer\.

**Evaluation period**  
The metric must meet or cross the threshold value for the full length of the evaluation period before starting a scaling event\. The evaluation period length is consecutive; if the metric retreats from the threshold, the evaluation period starts over again\.

**Adjustment type and value**  
This set of variables works together to specify how GameLift should adjust the fleet's capacity when a scaling event starts\. Choose from three possible adjustment types:  
+ **Change in capacity** – Increase or decrease the current capacity by a specified number of instances\. Set the adjustment value to the number of instances to add or remove from the fleet\. Positive values add instances, while negative values remove instances\. For example, a value of "\-10" scales down the fleet by 10 instances, regardless of the fleet's total size\.
+ **Percent change in capacity** – Increase or decrease the current capacity by a specified percentage\. Set the adjustment value to the percentage that you want to increase or decrease the fleet capacity by\. Positive values add instances, while negative values remove instances\. For example, for a fleet with 50 instances, a percentage change of "20" adds 10 instances to the fleet\.
+ **Exact capacity** – Increase or decrease the current capacity to a specific value\. Set the adjustment value to the exact number of instances that you want to maintain in the fleet\.

## Tips for rule\-based auto scaling<a name="fleets-autoscaling-rule-tips"></a>

The following suggestions can help you get the most out of auto scaling with rule\-based policies\.

### Use multiple policies<a name="fleets-autoscaling-policy-tips-multiples"></a>

You can have multiple auto scaling policies for a fleet at the same time\. The most common scenario is to have a target\-based policy manage most scaling needs and use rule\-based policies to handle edge cases\. There are no limits on using multiple policies\.

With multiple policies, each policy behaves independently\. There is no way to control the sequence of scaling events\. For example, if you have multiple policies driving scaling up, it's possible that player activity could start multiple scaling events simultaneously\. Avoid policies that start each other\. For example, you could create an infinite loop if you create scale\-up and scale\-down policies that set capacity beyond the threshold of each other\.

### Set maximum and minimum capacity<a name="fleets-autoscaling-policy-tips-maximums"></a>

Each fleet has a maximum and minimum capacity limit\. This feature is important when using auto scaling\. Auto scaling never sets capacity to a value outside of this range\. By default, newly created fleets have a minimum of 0 and a maximum of 1\. For your auto scaling policy to affect capacity as intended, increase the maximum value\.

Fleet capacity is also constrained by limits on the fleet's instance type and by service quotas in your AWS account\. You can't set a minimum and maximum outside these limits and account quotas\.

### Track metrics after a change in capacity<a name="fleets-autoscaling-policy-tips-cooldown"></a>

After changing capacity in response to an auto scaling policy, GameLift waits 10 minutes before responding to triggers from the same policy\. This wait gives GameLift time to add the new instances, launch the game servers, connect players, and start collecting data from the new instances\. During this time, GameLift evaluates the policy against the metric and tracks the policy's evaluation period, which restarts after a scaling event occurs\. This means that a scaling policy could start another scaling event immediately after the wait time is over\.

There is no wait time between scaling events that different auto scaling policies start\.