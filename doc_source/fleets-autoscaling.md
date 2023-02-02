# Auto\-scale fleet capacity with GameLift<a name="fleets-autoscaling"></a>

Use auto\-scaling in GameLift to dynamically scale your fleet capacity in response to game server activity\. As players arrive and start game sessions, auto scaling can add more instances; as player demand wanes, auto scaling can terminate unneeded instances\. Auto scaling is an effective way to minimize your hosting resources and costs, while still providing a smooth, fast player experience\.

To use auto scaling, you create scaling policies that tell GameLift when to scale up or down\. There are two types of scaling policies: target\-based and rule\-based\. The target\-based approach—target tracking—is a complete solution\. We recommend it as the simplest and most effective option\. Rule\-based scaling policies require you to define each aspect of the auto scaling decision\-making process, which is useful for addressing specific issues\. This solution works best as a supplement to target\-based auto scaling\.

You can manage target\-based auto scaling using the GameLift console, the AWS Command Line Interface \(AWS CLI\), or an AWS SDK\. You can manage rule\-based auto scaling using the AWS CLI or an AWS SDK only, although you can view rule\-based scaling policies in the console\.

**Topics**
+ [Target\-based auto scaling](fleets-autoscaling-target.md)
+ [Auto scale with rule\-based policies](fleets-autoscaling-rule.md)