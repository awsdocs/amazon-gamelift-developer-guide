# GameLift identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify GameLift resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating policies on the JSON tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the GameLift console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Allow player access for game sessions](#security_iam_id-based-policy-examples-player-access)
+ [Allow access to one GameLift queue](#security_iam_id-based-policy-examples-access-one-bucket)
+ [View GameLift fleets based on tags](#security_iam_id-based-policy-examples-view-fleet-tags)
+ [Access a game build file in Amazon S3](#security_iam_id-based-policy-examples-access-storage-loc)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete GameLift resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started using AWS managed policies** – To start using GameLift quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get started using permissions with AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant least privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for sensitive operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use policy conditions for extra security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Using the GameLift console<a name="security_iam_id-based-policy-examples-console"></a>

To access the GameLift console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the GameLift resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\.

To ensure that those entities can still use the GameLift console, add an inline policy to users and groups with the following policy syntax\. For more information, see [Adding permissions to a user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\. You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API, such as players using game clients\. Instead, allow access to only the actions that match the API operation that you're trying to perform\. 
+ Permissions required to use all GameLift console features: see inline policy syntax for administrators in [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\.

## Allow users to view their own permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

## Allow player access for game sessions<a name="security_iam_id-based-policy-examples-player-access"></a>

You want to grant access that is needed by game clients \(or client services that manage requests from game clients\) to create new game sessions and request player placement in an available game session\. There are several ways that your game might accomplish this task, including using game session placements with queues, matchmaking, or manual placement\. To view policy examples for these scenarios, see the inline policy syntax for players in [IAM policy examples for GameLift](gamelift-iam-policy-examples.md)\.

## Allow access to one GameLift queue<a name="security_iam_id-based-policy-examples-access-one-bucket"></a>

In this example, you want to grant an IAM user in your AWS account access to one of your GameLift queues, `gamesessionqueue/examplequeue123`, including the ability to add, update, and delete queue destinations\. 

This policy grants permissions to the user for the following actions: `gamelift:UpdateGameSessionQueue`, `gamelift:DeleteGameSessionQueue`, and `gamelift:DescribeGameSessionQueues`\. As shown, this policy uses the resource element to limit access to a single queue\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"ViewSpecificQueueInfo",
         "Effect":"Allow",
         "Action":[
            "gamelift:DescribeGameSessionQueues"
         ],
         "Resource":"arn:aws:gamelift:::gamesessionqueue/examplequeue123"
      },
      {
         "Sid":"ManageSpecificQueue",
         "Effect":"Allow",
         "Action":[
            "gamelift:UpdateGameSessionQueue",
            "gamelift:DeleteGameSessionQueue"
         ],
         "Resource":"arn:aws:gamelift:::gamesessionqueue/examplequeue123"
      }
   ]
}
```

## View GameLift fleets based on tags<a name="security_iam_id-based-policy-examples-view-fleet-tags"></a>

You can use conditions in your identity\-based policy to control access to GameLift resources based on tags\. This example shows how you might create a policy that allows viewing a fleet\. However, permission is granted only if the fleet tag `Owner` has the value of that user's user name\. This policy also grants the permissions necessary to complete this action on the console\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListFleetsInConsole",
            "Effect": "Allow",
            "Action": "gamelift:ListFleets",
            "Resource": "*"
        },
        {
            "Sid": "ViewFleetIfOwner",
            "Effect": "Allow",
            "Action": "gamelift:DescribeFleetAttributes",
            "Resource": "arn:aws:gamelift:*:*:fleet/*",
            "Condition": {
                "StringEquals": {"gamelift:ResourceTag/Owner": "${aws:username}"}
            }
        }
    ]
}
```

You can attach this policy to the IAM users in your account\. If a user named `richard-roe` attempts to view a GameLift fleet, the fleet must be tagged `Owner=richard-roe` or `owner=richard-roe`\. Otherwise he is denied access\. The condition tag key `Owner` matches both `Owner` and `owner` because condition key names are not case\-sensitive\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Access a game build file in Amazon S3<a name="security_iam_id-based-policy-examples-access-storage-loc"></a>

Once your game server has been integrated with GameLift, you must upload the build files to the managed GameLift service\. You must grant permissions to GameLift to access the build files in Amazon S3\. You can do this by attaching the following policy to an IAM role:" 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": arn:aws:s3:::bucket-name/object-name"
        }
    ]
}
```

For more information about uploading GameLift game files, see [Upload a custom server build to GameLift](gamelift-build-cli-uploading.md)\. 