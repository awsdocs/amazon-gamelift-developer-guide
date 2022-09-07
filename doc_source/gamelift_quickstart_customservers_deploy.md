# Deploy your GameLift resources<a name="gamelift_quickstart_customservers_deploy"></a>

To deploy your GameLift resources, complete the following tasks:
+ **Package and upload your custom game server build to GameLift**\. Upload your build to your fleet's [home AWS Region](gamelift-regions.md#gamelift-regions-hosting), and GameLift automatically distributes the build to any additional locations that you choose\. For more information, see [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md)\.
+ **Design a GameLift fleet configuration for your game**\. Choose the type of computing resources to use, which locations to deploy to, whether to use queues, and other options\. For more information, see [GameLift fleet design guide](fleets-design.md)\.
+ **Create fleets and deploy them with your custom game server**\. When a fleet is active, it's ready to host game sessions and accept players\. For more information, see [Create a new GameLift fleet](fleets-creating-all.md)\.
+ **Experiment with GameLift fleet configuration settings to optimize usage of your fleet resources**\. Adjust the number of game sessions to run concurrently on each instance\. Or, if your activation uses extensive CPU and memory resources, set game session activation limits\. For more information, see [GameLift fleet design guide](fleets-design.md)\. If you need to debug issues with an instance, see [Remotely access GameLift fleet instances](fleets-remote-access.md)\. For a tool that simplifies remotely accessing instances, see [amazon\-gamelift\-remote\-plus](https://github.com/aws-samples/amazon-gamelift-remote-plus) on GitHub\.
+ **Create a queue**\. Your queue manages how GameLift places new game sessions with available hosting resources\. It also implements your Spot Instance usage strategy\. For more information, see [Design a game session queue](queues-design.md)\.
+ **Use automatic scaling to manage your fleet's hosting capacity for expected player demand**\. For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\.
+ **\(Optional\) Set up a GameLift FlexMatch matchmaker with a set of custom matchmaking rules for your game**\. For more information, see [FlexMatch integration with GameLift hosting](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html)\.

To reduce any manual errors and to simplify the deployment of your global GameLift resources, we recommend that you use [infrastructure as code \(IaC\)](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html) to define all the resources\. GameLift hosting supports AWS CloudFormation so that you can define your resources with YAML or JSON templates\. You can deploy one or more stacks of those resources across multiple Regions\. You can also parameterize the template for any deployment\-specific configurations\.

In addition to using IaC, to manage the deployment of your AWS CloudFormation stacks, we recommend that you use a continuous integration and continuous delivery \(CI/CD\) tool such as AWS CodePipeline or open\-source tools such as Jenkins\. This helps you deploy either automatically or with an approval step whenever your game server binary is built\.

Common steps to automate with a CI/CD tool for deployment of GameLift resources for a new game server version include:
+ Building and testing your game server binary\.
+ Uploading the binary to GameLift using the [AWS SDK](http://aws.amazon.com/developer/tools/#sdk)\.
+ Deploying new GameLift fleets using the new build with AWS CloudFormation\.
+ After successfully deploying the new fleets, removing previous fleets from your GameLift queue and replacing them with the new fleets\.
+ After the previous fleets successfully end all game sessions, deleting the AWS CloudFormation stacks of these fleets\.

Alternatively, you can use the AWS Cloud Development Kit \(AWS CDK\) to define your GameLift resources with TypeScript, Python, or another supported programming language\. To deploy and update your resources, you can use the generated AWS CloudFormation templates with the [AWS Command Line Interface \(AWS CLI\)](http://aws.amazon.com/cli/)\.