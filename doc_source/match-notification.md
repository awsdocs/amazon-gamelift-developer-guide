# Set up FlexMatch Event Notification<a name="match-notification"></a>

If you're using FlexMatch matchmaking in your game, you need a way to track the status of individual matchmaking requests and take action as appropriate\. Sometimes, such as when players are required to accept a proposed match, these actions are time sensitive\. The simplest method for tracking each request's status is to continuously poll using [DescribeMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html)\. However, for a faster solution, we recommend using notifications to track matchmaking events\. Event notifications are simple to set up and are significantly faster and more efficient\.

There are two options for setting up event notifications\. You can use Amazon CloudWatch Events, which has a suite of tools available for managing events and taking action on them\. Alternatively, you can set up your own SNS topic\(s\) and attach them to your matchmaker to receive matchmaking event notifications directly\. 

See the list of FlexMatch events emitted by Amazon GameLift in [FlexMatch Matchmaking Events](match-events.md)\. 

## Set up CloudWatch Events<a name="match-notification-cwe"></a>

Amazon GameLift automatically posts all matchmaking events to CloudWatch Events\. With CloudWatch Events you can set up rules to have matchmaking events routed to a range of targets, including SNS topics and other AWS services for processing\. For example, you might set a rule to route the event "PotentialMatchCreated" to an AWS Lambda function that handles player acceptances\. Learn more about how to use CloudWatch Events in the [Getting Started](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_GettingStarted.html) guide, which includes a collection of tutorials\.

If you plan to use CloudWatch Events, when configuring your matchmakers, you can leave the notification target field empty, or reference an SNS topic if you want to use both options\. 

To access Amazon GameLift matchmaking events in CloudWatch Events, go to the [Amazon CloudWatch console](https://aws.amazon.com/cloudwatch) and open **Events**\. Be sure that you're in the region where you've set up your matchmaking configuration\. For more information about getting account credentials to access CloudWatch Events, see [Sign in to the Amazon CloudWatch Console](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/GettingSetup_cwe.html)\. Each matchmaking event is identified by the service \(GameLift\), the matchmaking name, and the matchmaking ticket\.

## Set up an SNS Topic<a name="match-notification-sns"></a>

You can ask Amazon GameLift to publish all events generated by a FlexMatch matchmaker to an Amazon Simple Notification Service \(SNS\) topic\. When configuring the matchmaker, set the notification target field to an SNS topic ARN\. 

**To set up an SNS topic for Amazon GameLift event notification**

1. Go to the [Amazon Simple Notification Service console](https://aws.amazon.com/sns)\.

1. **Create a topic\.** From the SNS dashboard, choose **Create topic** and follow the instructions to create your topic\. When the topic is created, the console automatically opens the **Topic details** page for the new topic\. 

1. **Allow Amazon GameLift to publish to the topic\.** If you're not already in the Topic details page for your topic, choose Topics from the navigation bar and click the topics ARN to open it\. Choose the topic action **Edit topic policy**, and go to the **Advanced view tab**\.

   Add the bolded syntax below to the end of your existing policy\. \(The entire policy is shown for clarity\.\)

   ```
   {
     "Version": "2008-10-17",
     "Id": "__default_policy_ID",
     "Statement": [
       {
         "Sid": "__default_statement_ID",
         "Effect": "Allow",
         "Principal": {
           "AWS": "*"
         },
         "Action": [
           "SNS:GetTopicAttributes",
           "SNS:SetTopicAttributes",
           "SNS:AddPermission",
           "SNS:RemovePermission",
           "SNS:DeleteTopic",
           "SNS:Subscribe",
           "SNS:ListSubscriptionsByTopic",
           "SNS:Publish",
           "SNS:Receive"
         ],
         "Resource": "arn:aws:sns:your_region:your_account:your_topic_name",
         "Condition": {
           "StringEquals": {
             "AWS:SourceOwner": "your_account"
           }
         }
       },
       {
         "Sid": "__console_pub_0",
         "Effect": "Allow",
         "Principal": { 
           "Service": "gamelift.amazonaws.com" 
         },
         "Action": "SNS:Publish",
         "Resource": "arn:aws:sns:your_region:your_account:your_topic_name"
       }
     ]
   }
   ```