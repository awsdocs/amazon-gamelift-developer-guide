# Deploy your Amazon GameLift resources<a name="gamelift_quickstart_customservers_deploy"></a>

To deploy your Amazon GameLift resources, complete the following tasks:
+ **Package and upload your custom game server build to the GameLift service**\. You only need to upload to the home Region of your fleet, and it will automatically be distributed to any additional locations that you choose\. For more information, see [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md)\.
+ **Design a GameLift fleet configuration for your game**\. Decide, for example, the type of computing resources to use, which locations to deploy to, whether to use queues, and other options\. For more information, see [GameLift fleet design guide](fleets-design.md)\.
+ **Create fleets and deploy them with your custom game server**\. When a fleet is active, it is ready to host game sessions and accept players\. For more information, see [Create a new GameLift fleet](fleets-creating-all.md)\. 
+ **Experiment with GameLift fleet configuration settings and refine as needed to optimize usage of your fleet resources**\. Adjust the number of game sessions to run concurrently on each instance, or set game session activation limits if your activation uses CPU and memory resources extensively\. For more information, see [GameLift fleet design guide](fleets-design.md)\. If you need to debug issues on the instance itself, see [Remotely access GameLift fleet instances](fleets-remote-access.md)\. See [ amazon\-gamelift\-remote\-plus](https://github.com/aws-samples/amazon-gamelift-remote-plus) for a tool that that simplifies the remote access\. 
+ **Create a queue to manage how new game sessions are placed with available hosting resources and to implement your Spot Instance strategy**\. For more information, see [Design a game session queue](queues-design.md)\. 
+ **Enable automatic scaling to manage your fleet's hosting capacity for expected player demand**\. For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\. 
+ **\[optional\] Set up a FlexMatch matchmaker with a set of custom matchmaking rules for your game**\. For more information, see [FlexMatch integration roadmap](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-tasks.html) \. 

To reduce any manual errors and to simplify the global deployment of your GameLift resources, we recommend you use [ Infrastructure as code](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/infrastructure-as-code.html) to define all of the resources\. GameLift hosting supports AWS CloudFormation so you can define your resources with YAML or JSON templates and deploy one or more stacks of those resources across multiple Regions\. You can parameterize the template for any deployment\-specific configurations\. 

In addition to Infrastructure as Code, we recommend you use a Continuous Integration / Continuous Delivery \(CI/CD\) tool like AWS CodePipeline or open\-source tools such as Jenkins to manage the deployment of your AWS CloudFormation stacks\. This will help you deploy either automatically or with an approval step whenever your game server binary is built\. 

Common automated steps of GameLift resources deployment for a new game server version with a CI/CD tool include:
+ Building and testing your game server binary
+ Uploading the binary to GameLift using the AWS SDK
+ Deploying new GameLift fleet\(s\) using the new build with AWS CloudFormation
+ After the new fleet is successfully deployed, removing the previous version fleet\(s\) from your GameLift queue and replacing with the new fleet\(s\)
+ After the old fleet\(s\) have successfully terminated all game sessions, deleting the AWS CloudFormation stack\(s\) of these fleet\(s\)

Alternatively, you can use the AWS Cloud Development Kit \(AWS CDK\) to define your GameLift resources with TypeScript, Python, or other supported programming language\. You can use the AWS CLI to deploy and update the resources with generated AWS CloudFormation templates\.