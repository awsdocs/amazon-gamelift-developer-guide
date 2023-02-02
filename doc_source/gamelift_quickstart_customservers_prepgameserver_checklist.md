# Onboarding<a name="gamelift_quickstart_customservers_prepgameserver_checklist"></a>

Use the following checklist to track items for onboarding your game for GameLift hosting\. Items marked **\[Critical\]** are critical for your production launch\.
+ **\[Critical\]** Fill out the GameLift onboarding questionnaire in the [GameLift console](https://console.aws.amazon.com/gamelift/)\.
+ **\[Critical\]** [Design and implement a backend service](gamelift_quickstart_customservers_designbackend.md) for game clients to interact with your game servers\.
+ **\[Critical\]** [Create AWS Identity and Access Management \(IAM\) roles](setting-up-aws-login.md) that you provide to GameLift server instances for access to other AWS resources\.
+ **\[Critical\]** [Design and implement failover to other AWS Regions](gamelift-regions.md) for FlexMatch and queues\.
+ [Plan the rollout of fleets to your target locations](gamelift-regions.md), considering your game's queue and fleet structure\.
+ [Automate your deployment](resources-cloudformation.md) using infrastructure as code \(IaC\) with AWS CloudFormation and the AWS Cloud Development Kit \(AWS CDK\)\.
+ [Collect logs and analytics](monitoring-overview.md) using Amazon CloudWatch and Amazon Simple Storage Service \(Amazon S3\)\.