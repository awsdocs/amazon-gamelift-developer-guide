# Target\-based auto scaling<a name="fleets-autoscaling-target"></a>

Target\-based auto scaling for GameLift adjusts capacity levels based on the fleet metric `PercentAvailableGameSessions`\. This metric represents the fleet's available buffer for sudden increases in player demand\.

The primary reason for maintaining a capacity buffer is player wait time\. When game session slots are ready and waiting, it takes seconds to get new players into game sessions\. If no resources are available, players must wait for existing game sessions to end or for new resources to become available\. It can take minutes to start up new instances and server processes\.

When setting up target\-based auto scaling, specify the size of the buffer that you want the fleet to maintain\. Because `PercentAvailableGameSessions` measures the percentage of available resources, the actual buffer size is a percentage of the total fleet capacity\. GameLift adds or removes instances to maintain the target buffer size\. With a large buffer, you minimize wait time, but you also pay for extra resources that you may not use\. If your players are more tolerant of wait times, you can lower costs by setting a small buffer\.

## To set target\-based auto scaling<a name="fleets-autoscaling-policy-setting-console"></a>

------
#### [ Console ]

1. Open the [Amazon GameLift console](https://console.aws.amazon.com/gamelift/)\.

1. In the navigation pane, choose **Hosting**, **Fleets**\.

1. On the **Fleets** page, choose the name of an active fleet to open the fleet's detail page\.

1. Choose the **Scaling** tab\. This tab displays the fleet's historical scaling metrics and contains controls for adjusting current scaling settings\. 

1. Under **Scaling capacity**, check that the **Min size** and **Max size** limits are appropriate for the fleet\. With auto scaling enabled, capacity adjusts between these two limits\.

1. In **Target\-based auto\-scaling policy**, choose **Edit**\.

1. In the **Edit target\-based auto\-scaling policy** dialog box, for **Percent available game sessions**, set the percentage that you want to maintain, and then choose **Confirm**\. After you've confirmed the settings, GameLift adds a new target\-based policy under **Target\-based auto\-scaling policy**\.

------
#### [ AWS CLI ]

1. **Set capacity limits\.** Set the limit values using the [update\-fleet\-capacity](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-capacity.html) command\. For more information, see [Set GameLift capacity limits](fleets-capacity-limits.md)\.

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
   aws gamelift put-scaling-policy \
       --fleet-id "fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa" \
       --name "My_Target_Policy_1" \
       --policy-type "TargetBased" \
       --metric-name "PercentAvailableGameSessions" \
       --target-configuration "TargetValue=5"
   ```

------