# Auto\-Scale with Target Tracking<a name="fleets-autoscaling-target"></a>

Target tracking adjusts capacity levels based on a single key fleet metric: "percent available game sessions"\. This metric measures the number of available game session slots at current capacity—additional game sessions that could be started immediately\. In effect, this metric represents the fleet's buffer against sudden increases in player demand\. 

The primary reason for maintaining a capacity buffer is player wait time\. When game session slots are ready and waiting, the time it takes to get new players into game sessions can be measured in seconds\. If no resources are available, players must wait for existing game sessions to end or for new resources to become available\. The time required to start up new instances and server processes can be minutes\. 

When setting up target tracking, you simply specify the size of the buffer you want the fleet to maintain\. Since the metric "percent available game sessions" measures the percentage of available resources, the actual buffer size is a percentage of the total fleet capacity\. Amazon GameLift adds or removes as many instances as are needed to maintain the target buffer size\. Choosing a buffer size depends on how you want to prioritize minimizing player wait time against controlling hosting costs\. With a large buffer, you minimize wait time but you also pay for extra resources that may not get used\. If your players are more tolerant of wait times, you can lower costs by setting a small buffer\. 

## Set Target Tracking \(Console\)<a name="fleets-autoscaling-policy-setting-console"></a>

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. On the **Fleets** page, click the name of an active fleet to open the fleet's detail page\. \(You can also access a fleet's detail page via the **Dashboard**\.\) 

1. Open the **Scaling** tab\. This tab displays the fleet's historical scaling metrics and contains controls for adjusting current scaling settings\. Scaling settings are located below the metrics graph\.

1. Under **Instance Limits**, check that the minimum and maximum limits are appropriate for the fleet\. With auto\-scaling enabled, capacity may adjust to any level between these two limits\. 

1. Under **Auto\-Scaling Policies**, check the option to **Maintain a buffer of X percent game session availability**\. Set a buffer size, and click the checkmark button ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/checkmark.png) to save the auto\-scaling settings\. Once you've saved the settings, a new target\-based policy is added to the **Scaling policies **table\. 

1. To turn on auto\-scaling for the fleet\. verify that the option to **Disable all scaling policies in the fleet** is unchecked\. If this option is checked, all policies, including the new target\-tracking policy, are disabled\. This state is reflected in the **Scaling policies** table\.

## Set Target Tracking \(AWS CLI\)<a name="fleets-autoscaling-policy-setting-cli"></a>

1. **Set capacity limits\.** Set either or both limit values using the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command\. For help, see [To Set Capacity Limits \(AWS CLI\)](fleets-capacity-limits.md#fleets-capacity-limits-cli)\.

1. **Create a new policy\.** Open a command\-line window and use the [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/gamelift/put-scaling-policy.html) command with your policy's parameter settings\. To update an existing policy, specify the policy's name and provide a complete version of the updated policy\.

   ```
   --fleet-id <unique fleet identifier>
   --name "<unique policy name>"
   --policy-type <target- or rule-based policy>
   --metric-name <name of metric>
   --target-configuration <buffer size>
   ```

   Example:

   ```
   $aws gamelift put-scaling-policy
   --fleet-id "fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa"
   --name "My_Target_Policy_1"
   --policy-type "TargetBased" 
   --metric-name "PercentAvailableGameSessions"
   --target-configuration "TargetValue=5"
   ```

   *Copyable version:*

   ```
   $aws gamelift put-scaling-policy --fleet-id "fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa" --name "My_Target_Policy_1" --policy-type "TargetBased" --metric-name "PercentAvailableGameSessions" --target-configuration "TargetValue=5"
   ```