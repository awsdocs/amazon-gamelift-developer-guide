# Auto\-scale fleet capacity with GameLift<a name="fleets-autoscaling"></a>

Use auto\-scaling to dynamically scale your fleet capacity in response to game server activity\. As players arrive and start game sessions, auto\-scaling can add more instances; as player demand wanes, auto\-scaling can terminate unneeded instances\. Auto\-scaling is an effective way to minimize your hosting resources and costs, while still providing a smooth and fast player experience\. Learn more about [how auto\-scaling](gamelift-howitworks.md#gamelift-howitworks-autoscale) works in GameLift\.

Auto\-scaling is done by creating scaling policies that provide instructions to GameLift for scaling up or down\. There are two types of scaling policies, target\-based and rule\-based\. The target\-based approach—target tracking—offers a complete solution; it is recommended as the simplest and most effective option\. Rule\-based scaling policies, which require you to define each aspect of the auto\-scaling decision\-making process, is useful for addressing specific issues\. It works best as a supplement to target\-based auto\-scaling\.

Target\-based auto\-scaling can be managed using either the GameLift Console or the AWS CLI or AWS SDK\. Rule\-based auto\-scaling is managed using the AWS CLI or AWS SDK only, although you can view rule\-based scaling policies in the Console\.

**Topics**
+ [Auto\-scale with target tracking](fleets-autoscaling-target.md)
+ [Auto\-scale with rule\-based policies](fleets-autoscaling-rule.md)