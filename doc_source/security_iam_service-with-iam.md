# How GameLift works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to GameLift, you should understand what IAM features are available to use with GameLift\. To get a high\-level view of how GameLift and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

If you're using GameLift FleetIQ as a standalone feature with Amazon EC2, also refer to [Security in Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## GameLift identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. GameLift supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Policy actions in GameLift use the following prefix before the action: `gamelift:`\. For example, to grant permission to request a new game session with the GameLift`StartGameSessionPlacement` API operation, you include the `gamelift:StartGameSessionPlacement` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. GameLift defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "gamelift:action1",
      "gamelift:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "gamelift:Describe*"
```

To see a list of GameLift actions, see [Actions Defined by Amazon GameLift](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazongamelift.html#amazongamelift-actions-as-permissions) in the *IAM User Guide*\.

The following GameLift actions have additional permission dependencies:
+ The VPC peering actions `CreateVpcPeeringAuthorization` and `CreateVpcPeeringConnection` require access to the relevant VPC\. The VPC can be owned by the same AWS account that you're using to manage GameLift, or it can be owned by a different AWS account\. For more information on VPC peering with GameLift, see [VPC peering for GameLift](vpc-peering.md)\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```



Long\-lived GameLift resources have ARN values, which allows the resources to have their access managed using IAM policies\. The GameLift fleet resource has an ARN with the following syntax:

```
arn:${Partition}:gamelift:${Region}:${Account}:fleet/${FleetId} 
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, to specify the `fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa` fleet in your statement, use the following ARN:

```
"Resource": "arn:aws:gamelift:us-west-2:123456789012:fleet/fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa"
```

To specify all fleets that belong to a specific account, use the wildcard \(\*\):

```
"Resource": "arn:aws:gamelift:us-west-2:123456789012:fleet/*"
```

Some GameLift actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

To view a list of GameLift resource types with ARNs, see [About GameLift hosting resources](resources-defined.md)\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by Amazon GameLift](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazongamelift.html#amazongamelift-actions-as-permissions)\.

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

GameLiftsupports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



 Many GameLift actions support the `aws:RequestedTag` condition key\. To see a list of GameLift condition keys, see [Condition Keys for Amazon GameLift](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazongamelift.html#amazongamelift-policy-keys) in the *IAM User Guide*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by Amazon GameLift](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazongamelift.html#amazongamelift-actions-as-permissions)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of GameLift identity\-based policies, see [GameLift identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## GameLift resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

GameLift does not support resource\-based policies\.

## Authorization based on GameLift tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to GameLift resources or pass tags in a request to GameLift\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `gamelift:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging GameLift resources, see [TagResource\(\)](https://docs.aws.amazon.com/gamelift/latest/apireference/API_TagResource.html) in the *Amazon GameLift API Reference*\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [View GameLift fleets based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-fleet-tags)\.

## GameLift IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with GameLift<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

GameLift supports using temporary credentials\. 

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

GameLift does not support service\-linked roles\. 

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

GameLift supports the use of service roles for the following scenarios:
+ Allow your GameLift\-hosted game servers to access other AWS resources, such as an AWS Lambda function or an Amazon DynamoDB database\. Because game servers are hosted on fleets that are managed by GameLift, you need a service role that gives GameLift limited access to your other AWS resources\. For more information, see [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.