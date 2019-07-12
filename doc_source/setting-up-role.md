# Set Up a Role for Amazon GameLift Access<a name="setting-up-role"></a>

Some GameLift features require you to extend limited access to your AWS resources\. This is done by creating an AWS Identity and Access Management \(IAM\) role\. A role specifies two things: \(1\) who can assume the role, and \(2\) which resources they can control while using the role\. This topic provides guidance on how to set up a role to extend access to the Amazon GameLift service\. 

**To set up an IAM role for the GameLift service**

By creating a role specifically for GameLift, you define which of your AWS resources can be accessed either by the GameLift service or by your applications \(such as game servers\) that are running on GameLift\.

Currently, a service\-specific role for Amazon GameLift must be manually constructed with inline permissions and trust policies\. You can update the role any time using the IAM service via the console or the AWS CLI\. 

1. Create an IAM role by using the AWS CLI\. \(The IAM console currently does not allow you to create a generic service role and add or edit policies\.\) See the IAM user guide topic [Creating a Role for a Service \(AWS CLI\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-cli) for specific instructions\. The role must be created under the AWS account that you use to manage GameLift, so make sure you're using the proper AWS account credentials\.

1. Create an inline permissions policy and attach it to the role\. The permissions policy is where you specify what level of access that is covered by the role\. You can specify access to a service or a resource, such as an Amazon S3 bucket, and you can limit permissions to specific actions\. You can opt to create separate IAM roles for different sets of permissions to limit vulnerability\. See these [Examples of Policies for Delegating Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_policy-examples.html)\. Once you've defined your policy syntax, attach it to the service as described in the instructions linked to in Step 1\.

1. Create a trust policy and attach it to the role\. A trust policy specifies which AWS service\(s\) can assume the role\. Use the following syntax: 

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

1. Once you've created the role, you can view it in the IAM console\. Make a note of the new role's ARN, as you may need to use it when setting up a GameLift feature\.