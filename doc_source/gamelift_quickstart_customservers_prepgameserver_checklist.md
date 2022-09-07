# Onboarding<a name="gamelift_quickstart_customservers_prepgameserver_checklist"></a>

Use the following checklists to keep track of onboarding items\. Items marked **\[Critical\]** are critical for your production launch\. 
+ **\[Critical\]** Design and implement a backend service for game clients to interact with your game servers\.
+ **\[Critical\]** Use IAM roles that you provide to GameLift server instances for access to other AWS resources\.
+ **\[Critical\]** Design and implement fail over to other Regions for FlexMatch and queues\.
+ Plan the roll\-out of fleets to your target Regions, with queue and fleet structure\.
+ Use Infrastructure as Code with AWS CloudFormation and AWS CDK to automate your deployment\.
+ Build a load test client and automate load testing\.
+ Test your system with alpha, beta, and soft launch tests\.
+ Collect logs and analytics using Amazon CloudWatch and Amazon S3\.