# Generating GameLift pricing estimates<a name="gamelift-calculator"></a>

With AWS Pricing Calculator, you can [create a pricing estimate for GameLift](https://calculator.aws/#/createCalculator/GameLift)\. You don't need an AWS account or in\-depth knowledge of AWS to use the calculator\.

AWS Pricing Calculator calculator guides you through the decisions that affect service costs to give you an idea of how much GameLift might cost for your game project\. If you're not yet sure how you plan to use GameLift, then use the default values to generate an estimate\. When planning for production usage, the calculator can help you test out potential scenarios and generate more accurate estimates\.

You can use AWS Pricing Calculator to generate estimates for the following GameLift hosting options:
+ [Estimate GameLift hosting](#gamelift-calculator-hosting)
+ [Estimate GameLift standalone FlexMatch](#gamelift-calculator-flexmatch)

## Estimate GameLift hosting<a name="gamelift-calculator-hosting"></a>

This option provides a cost estimate for hosting your games on GameLift managed servers, including the costs for server instance usage and data transfer\. FlexMatch matchmaking is included in the cost for GameLift managed hosting\.

If you are hosting or plan to host game servers in more than one AWS Region or on more than one instance type, create an estimate for each Region and instance type\.

### GameLift instances<a name="gamelift-calculator-hosting-instances"></a>

This section helps you estimate the type and number of compute resources that you need to host game sessions for your players\. GameLift uses [Amazon Elastic Compute Cloud \(Amazon EC2\) instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) to manage game servers\. In GameLift, you deploy a fleet of instances with a specific instance type and operating system\. If you have or plan to have multiple fleets, create an estimate for each fleet\.

To get started, open the [Configure Amazon GameLift page](https://calculator.aws/#/createCalculator/GameLift) of AWS Pricing Calculator\. Add a **Description**, choose a **Region**, and then choose **Estimate GameLift hosting \(Instance \+ Data Transfer Out\)**\. Under **GameLift instances**, complete the following fields:
+ **Peak concurrent players \(peak CCU\)**

  This is the maximum number of players who can connect to your game servers at the same time\. This field indicates how much hosting capacity GameLift needs to meet peak player demand\. Enter the daily peak number of players that you expect to host using instances in your chosen AWS Region\.

  For example, if you want to let 1,000 players connect to your game at any one time, keep the default value of **1000**\.
+ **Average CCU per hour as a percentage of peak daily CCU **

  This is the average number of concurrent players per hour over a 24\-hour period\. We use this value to estimate the amount of sustained hosting capacity that GameLift needs to maintain for your players\. If you're not sure what percentage value to use, keep the default value of **50** percent\. For games with stable player demand, we recommend entering a value of **70** percent\.

  For example, if your game has an average hourly CCU of 6,000 and a peak CCU of 10,000, then enter the value of **60** percent\.
+ **Game sessions per instance**

  This is the number of game sessions that each of your game server instances can host concurrently\. Factors that can affect this number include the resource requirements of your game server, the number of players to host in each game session, and player performance expectations\. If you know the number of concurrent game sessions for your game, then enter that value\. Alternatively, keep the default value of **20**\.
+ **Players per game session**

  This is the average number of players who connect to a game session, as defined in your game design\. If you have game modes with different number of players, estimate an average number of players per game session across your entire game\. The default value is **8**\.
+ **Instance idle buffer %**

  This is the percentage of unused hosting capacity to maintain in reserve to handle sudden spikes in player demand\. Buffer size is a percentage of the total number of instances in a fleet\. The default value is **10** percent\.

  For example, with a 20 percent idle buffer, a fleet supporting players with 100 active instances maintains 20 idle instances\.
+ **Spot instance %**

  GameLift fleets can use a combination of On\-Demand Instances and Spot Instances\. While On\-Demand Instances offer more reliable availability, Spot Instances offer a highly cost\-efficient alternative\. We recommend using a combination to optimize both cost savings and availability\. For information about how GameLift uses Spot Instances, see [On\-Demand Instances versus Spot Instances](gamelift-ec2-instances.md#gamelift-ec2-instances-spot)\.

  For this field, enter the percentage of Spot Instances to maintain in a fleet\. We recommend a Spot Instance percentage between 50 and 85 percent\. The default value is **50** percent\.

  For example, if you deploy a fleet with 100 instances and specify **40** percent, GameLift works to maintain 60 On\-Demand Instances and 40 Spot Instances\. 
+ **Instance type**

  GameLift fleets can use a range of Amazon EC2 instance types that vary in computing power, memory, storage, and networking capabilities\. When you configure a GameLift fleet, choose an instance type that best fits your game's needs\. For information about selecting an instance type with GameLift, see [Choosing GameLift computing resources](gamelift-ec2-instances.md)\.

  If you know the instance type that you're using or plan to use in your GameLift fleet, choose that type\. If you're not sure what type to choose, consider choosing **c5\.large**\. This is a high\-availability type with average size and capabilities\.
+ **Operating system**

  This field specifies the operating system that your game servers run on—either Linux or Windows\. The default value is **Linux**\.

### Data transfer out \(DTO\)<a name="gamelift-calculator-hosting-dto"></a>

This section helps you estimate the cost for traffic between your game clients and the game servers\. Data transfer fees apply to outbound traffic only\. Inbound data transfer has no cost\.

On the [Configure Amazon GameLift page](https://calculator.aws/#/createCalculator/GameLift) of AWS Pricing Calculator, expand **Data transfer out \(DTO\)**, and then complete the following fields:
+ **DTO estimate type**

  You can choose to estimate DTO in either of the following two ways, depending on how you track data transfer for your game\.
  + **Per month \(in GB\)** – If you track monthly traffic for your game servers, choose this type\.
  + **Per player** – If you track data transfer by player, choose this type\. This is the default type\.

    In the following field, you estimate per\-player DTO based on the number of player hours that you calculated in the previous section\.
+ **DTO per month \(in GB\)**

  If you chose the **Per month \(in GB\)** DTO estimate type, then enter your estimated monthly DTO usage in GB from each instance, per Region\.
+ **DTO per player**

  If you chose the **Per player** DTO estimate type, then enter your game's estimated DTO usage per player in KB/sec\. The default value is **4**\.

When you're done configuring your GameLift pricing estimate, choose **Add to my estimate**\. For more information about creating and managing estimates in AWS Pricing Calculator, see [Create an estimate, configure a service, and add more services](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/create-estimate.html) in the *AWS Pricing Calculator User Guide*\.

## Estimate GameLift standalone FlexMatch<a name="gamelift-calculator-flexmatch"></a>

This option provides a cost estimate for using FlexMatch matchmaking as a standalone service while hosting your games with another game server solution\. This includes GameLift self\-managed hosting with FleetIQ and on\-premises hosting, peer\-to\-peer, or cloud compute primitive data types\. Standalone FlexMatch costs are based on the computing power used\.

If you have or plan to have more than one matchmaker in different AWS Regions, create an estimate for each Region\.

**Note**  
GameLift FlexMatch is available in the following Regions: US East \(N\.Virginia\), US West \(Oregon\), Asia Pacific \(Seoul\), Asia Pacific \(Sydney\), Asia Pacific \(Tokyo\), Europe \(Frankfurt\), Europe \(Ireland\)\.

To get started, open the [Configure Amazon GameLift page](https://calculator.aws/#/createCalculator/GameLift) of AWS Pricing Calculator\. Add a **Description**, choose a **Region**, and then choose **Estimate GameLift Standalone FlexMatch**\. Under **Amazon GameLift FlexMatch**, complete the following fields:
+ **Peak concurrent players \(peak CCU\)**

  This is the maximum number of players who can connect to your game servers at the same time and request matchmaking\. Enter the daily peak number of players that you expect to match into game sessions in your chosen Region\. 

  For example, if you want to match as many as 1,000 players at a time, keep the default value of **1000**\.
+ **Average CCU per hour as a percentage of peak daily CCU**

  This is the average number of concurrent players per hour over a 24\-hour period\. This value helps estimate the volume of your matchmaking requests\. If you're not sure what percentage value to use, keep the default value of **50** percent\. For games with stable player demand, we recommend entering a value of **70** percent\.

  For example, if your game has an average hourly CCU of 6,000 and a peak CCU of 10,000, then enter the value of **60** percent\.
+ **Number of players per match**

  This is the average number of players who match to a game session, as defined in your game design\. If you have game modes with different numbers of players, estimate an average number of players per game session across your entire game\. The default value is **8**\.
+ **Game duration \(in minutes\)**

  This is the average length of time that players remain in a game session from start to finish\. This value helps determine how often players might require a new match\. Enter an average game duration in minutes for your players\. The default value is **1**\.
+ **Matchmaking rule complexity**

  *Matchmaking rule complexity* refers to the number and type of rules that you use to match players\. The level of complexity of your rule set helps determine the amount of computing power required for each match\.
  + **Lower complexity** – Choose this option if your matchmaking rule set includes few rules, uses simpler rule types \(such as comparison rules\), and has rules that form successful matches with fewer attempts\.
  + **Higher complexity** – Choose this option if your matchmaking rule set includes multiple rules, uses more complex rule types \(such as distance or latency rules\), and has restrictive rules that result in more failures and require more matching attempts\.

  For more information about rule complexity and pricing, see [Amazon GameLift FlexMatch](http://aws.amazon.com/gamelift/pricing/#Amazon_GameLift_FlexMatch) on the Amazon GameLift Pricing page\.

When you're done configuring your GameLift FlexMatch pricing estimate, choose **Add to my estimate**\. For more information about creating and managing estimates in AWS Pricing Calculator, see [Create an estimate, configure a service, and add more services](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/create-estimate.html) in the *AWS Pricing Calculator User Guide*\.