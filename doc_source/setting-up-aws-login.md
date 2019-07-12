# Set Up an AWS Account<a name="setting-up-aws-login"></a>

Amazon GameLift is an AWS service, and you must have an AWS account to use Amazon GameLift\. Creating an AWS account is free\.

For more information on what you can do with an AWS account, see [Getting Started with AWS](https://aws.amazon.com/getting-started/)\.

**Set up your account for Amazon GameLift**

1. **Get an account\.** Open [Amazon Web Services](https://aws.amazon.com/) and choose **Sign In to the Console**\. Follow the prompts to either create a new account or sign into an existing one\.

1. **Set up user groups and access permissions\.** Open the AWS Identity and Access Management \(IAM\) service console and follow these steps to define a set of users or user groups and assign access permissions to them\. Permissions are extended to a user or user group by attaching an [IAM policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html), which specifies the set of AWS services and actions a user should have access to\. For detailed instructions on using the Console \(or the AWS CLI or other tools\) to set up your user groups, see [Creating IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)\.

   1. **Create an administrative user or user group**\. Administrative users include anyone who manages core Amazon GameLift resources, such as builds and fleets\. To set permissions, you must create your own policy from scratch\. [This example](gamelift-iam-policy-examples.md) illustrates an administrator policy for Amazon GameLift services\. 

   1. **Create a player user**\. A player user represents your game client\(s\)\. It enables access to Amazon GameLift client functionality, such as acquiring game session information and joining players to games\. Your game client must use the player user credentials when communicating with the Amazon GameLift service\. To set permissions, you must create your own policy from scratch\. [This example](gamelift-iam-policy-examples.md) illustrates a player policy for Amazon GameLift services\. 