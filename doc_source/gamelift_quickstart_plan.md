# Plan and deploy your GameLift resources<a name="gamelift_quickstart_plan"></a>

Use the following tips to help plan your global GameLift resources deployment\. For information about where you can host your games with GameLift, see [GameLift hosting in AWS Regions and Local Zones](gamelift-regions.md)\.

To deploy your Amazon GameLift resources, complete the following tasks:
+ **Package and upload your game server to GameLift or to your hardware\.** When uploading your server to GameLift, you upload it only to the home AWS Region of your fleet\. GameLift automatically distributes the server to other locations in the fleet\. For more information, see [Uploading builds and scripts to GameLift](gamelift-build-intro.md)\.
+ **Design and deploy a GameLift fleet for your game\.** Determine the type of computing resources to use, which locations to deploy to, whether to use queues, and other options\. For more information, see [GameLift fleet design guide](fleets-design.md)\.
+ **Create queues to manage new game session placements and Spot Instance strategies\.** For more information, see [Design a game session queue](queues-design.md)\.
+ **Use automatic scaling to manage your fleet's hosting capacity for expected player demand\.** For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\.
+ **Use FlexMatch matchmaking rules for your game\.** For more information, see [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.

## Automatically deploy your GameLift resources<a name="gamelift_quickstart_customservers_deploy"></a>

To streamline the global deployment of your GameLift resources, we recommend that you use [infrastructure as code \(IaC\)](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html) to define the resources\. Because GameLift supports AWS CloudFormation templates, you can set parameters in the templates for any deployment\-specific configurations\.

To manage the deployment of your AWS CloudFormation stacks, we also recommend using continuous integration and continuous delivery \(CI/CD\) tools and services such as AWS CodePipeline\. These help you deploy automatically or with approval whenever you build game server binary\.

The following are some common steps of GameLift resources deployment for a new game server version that you can automate using a CI/CD tool or service:
+ Building and testing your game server binary\.
+ Uploading the binary to GameLift or your hardware\.
+ Deploying new fleets in the new build\.
+ After you deploy the new fleets, removing the previous version fleets from your GameLift queue and replacing them with the new fleets\.
+ After the previous version fleets successfully end all game sessions, deleting the AWS CloudFormation stacks of those fleets\.

You can also use the AWS Cloud Development Kit \(AWS CDK\) to define your GameLift resources\. For more information about the AWS CDK, see the [AWS Cloud Development Kit \(AWS CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/v2/guide/)\.