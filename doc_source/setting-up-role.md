# Set up an IAM service role for GameLift<a name="setting-up-role"></a>

Some GameLift features require you to extend limited access to your AWS resources\. You can do this by creating an AWS Identity and Access Management \(IAM\) *service role*\.  A service role is an [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that a service assumes to perform actions on your behalf\. An IAM administrator can create, modify, and delete a service role from within IAM\. For more information, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 

This topic covers GameLift hosted solutions\. If you use GameLift FleetIQ to optimize game hosting on your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, see [Set up your AWS account for GameLift FleetIQ](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-iam-permissions.html)\.

For the following procedure, use these permissions policies:
+ **Permissions for GameLift to assume the service role**

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "Service": "gamelift.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
      }
    ]
  }
  ```
+ **Permissions to access AWS Regions that aren't enabled by default**

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "Service": [
            "gamelift.amazonaws.com",
            "gamelift.ap-east-1.amazonaws.com",
            "gamelift.me-south-1.amazonaws.com",
            "gamelift.af-south-1.amazonaws.com",
            "gamelift.eu-south-1.amazonaws.com" 
          ]
        },
        "Action": "sts:AssumeRole"
      }
    ]
  }
  ```

**To create a role for an AWS service \(IAM console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**, and then choose **Create role**\.

1. Choose the **AWS service** role type\.

1. Choose the use case for your service\. Use cases are defined by the service to include the trust policy that the service requires\.

1. Choose **Next**\.

1. If possible, select the policy to use for the permissions policy\. Otherwise, choose **Create policy** to open a new browser tab and create a new policy from scratch\. For more information, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\.

1. After you create the policy, close that tab and return to your original tab\. Select the check box next to the permissions policies that you want the service to have\.

   Depending on the use case that you selected, the service might let you do any of the following:
   + Nothing, because the service defines the permissions for the role\.
   + Choose from a limited set of permissions\.
   + Choose from any permissions\.
   + Select no policies at this time\. However, you can create the policies later, and then attach them to the role\.

1. \(Optional\) Set a [permissions boundary](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)\. This is an advanced feature that is available for service roles, but not for service\-linked roles\. 

   Expand the **Permissions boundary** section and choose **Use a permissions boundary to control the maximum role permissions**\. IAM includes a list of the AWS managed and customer managed policies in your account\. Select the policy to use for the permissions boundary or choose **Create policy** to open a new browser tab and create a new policy from scratch\. For more information, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\. After you create the policy, close that tab and return to your original tab to select the policy to use for the permissions boundary\.

1. Choose **Next**\.

1. For **Role name**, the degree of role name customization is defined by the service\. If the service defines the role's name, this option is not editable\. In other cases, the service might define a prefix for the role and let you enter an optional suffix\. Some services let you specify the entire name of your role\.

   If possible, enter a role name or role name suffix to help you identify the purpose of this role\. Role names must be unique within your AWS account\. Because role names are case insensitive, you cannot create roles named both **PRODROLE** and **prodrole**\. Various entities might reference the role\. Therefore, you cannot edit the name of the role after it has been created\.

1. \(Optional\) For **Description**, enter a description for the new role\.

1. Choose **Edit** in the **Step 1: Select trusted entities** or **Step 2: Select permissions** sections to edit the use cases and permissions for the role\. 

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Review the role, and then choose **Create role**\.