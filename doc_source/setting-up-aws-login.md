# Set up an AWS account<a name="setting-up-aws-login"></a>

To start using GameLift, create and set up your AWS account\. There's no charge to create an AWS account\. This section walks you through creating your account, setting up your users, and configuring permissions\.

**Topics**
+ [Get an AWS account and your root user credentials](#getting-started-signup)
+ [Creating an IAM user](#getting-started-create-iam-user)
+ [Signing in as an IAM user](#getting-started-signin-iam-user)
+ [Creating IAM user access keys](#getting-started-iam-user-access-keys)
+ [Creating a player user](#getting-started-iam-player-user)
+ [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)
+ [Set up an IAM service role for GameLift](setting-up-role.md)

## Get an AWS account and your root user credentials<a name="getting-started-signup"></a>

To access AWS, you must sign up for an AWS account\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

 AWS sends you a confirmation email after the sign\-up process is complete\. At any time, you can view your current account activity and manage your account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **My Account**\.

## Creating an IAM user<a name="getting-started-create-iam-user"></a>

If your account already includes an AWS Identity and Access Management \(IAM\) user with full AWS administrative permissions, you can skip this section\.

When you first create an AWS account, you begin with one sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\.

**Important**  
We strongly recommend that you don't use the root user for your everyday tasks\. Safeguard your root user credentials and use them to perform the tasks that only the root user can perform\. For the complete list of tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html) in the *AWS Account Management Reference Guide*\.

Administrative users include anyone who manages GameLift resources\. For instructions about giving a user GameLift administrator permissions, see [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\.

To create an administrator user, choose one of the following options\.


****  

| Choose one way to manage your administrator | To | By | You can also | 
| --- | --- | --- | --- | 
| In IAM Identity Center \(Recommended\) | Use short\-term credentials to access AWS\.This aligns with the security best practices\. For information about best practices, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-users-federation-idp) in the *IAM User Guide*\. | Following the instructions in [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide\. | Configure programmatic access by [Configuring the AWS CLI to use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) in the AWS Command Line Interface User Guide\. | 
| In IAM \(Not recommended\) | Use long\-term credentials to access AWS\. | Following the instructions in [Creating your first IAM admin user and user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the IAM User Guide\. | Configure programmatic access by [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

## Signing in as an IAM user<a name="getting-started-signin-iam-user"></a>

 Sign in to the [IAM console](https://console.aws.amazon.com/iam) by choosing **IAM user** and entering your AWS account ID or account alias\. On the next page, enter your IAM user name and your password\.

**Note**  
For your convenience, the AWS sign\-in page uses a browser cookie to remember your IAM user name and account information\. If you previously signed in as a different user, choose the sign\-in link beneath the button to return to the main sign\-in page\. From there, you can enter your AWS account ID or account alias to be redirected to the IAM user sign\-in page for your account\.

## Creating IAM user access keys<a name="getting-started-iam-user-access-keys"></a>

Users need programmatic access if they want to interact with AWS outside of the AWS Management Console\. The way to grant programmatic access depends on the type of user that's accessing AWS:
+ If you manage identities in IAM Identity Center, the AWS APIs require a profile, and the AWS Command Line Interface requires a profile or an environment variable\.
+ If you have IAM users, the AWS APIs and the AWS Command Line Interface require access keys\. Whenever possible, create temporary credentials that consist of an access key ID, a secret access key, and a security token that indicates when the credentials expire\.

To grant users programmatic access, choose one of the following options\.


****  

| Which user needs programmatic access? | To | By | 
| --- | --- | --- | 
|  Workforce identity \(Users managed in IAM Identity Center\)  | Use short\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\. |  Following the instructions for the interface that you want to use: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/setting-up-aws-login.html)  | 
| IAM | Use short\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\. | Following the instructions in [Using temporary credentials with AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html) in the IAM User Guide\. | 
| IAM | Use long\-term credentials to sign programmatic requests to the AWS CLI or AWS APIs \(directly or by using the AWS SDKs\)\.\(Not recommended\) | Following the instructions in [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

## Creating a player user<a name="getting-started-iam-player-user"></a>

A player user represents your game clients\. It enables access to GameLift client functionality, such as acquiring game session information and joining players to games\. Your game client must use player user credentials when communicating with GameLift\. For examples of player user permissions, see [Player policy examples](gamelift-iam-policy-examples.md#iam-policy-admin-game-dev-example)\.