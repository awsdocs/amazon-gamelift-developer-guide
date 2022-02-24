# Monitoring Amazon GameLift<a name="monitoring-overview"></a>

If you're using GameLift FleetIQ as a standalone feature with Amazon EC2, also refer to [Security in Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon GameLift and your other AWS solutions\. There are three primary uses for metrics with Amazon GameLift: to monitor system health and set up alarms, to track game server performance and usage, and to manage capacity using manual or auto\-scaling\. 

AWS provides the following monitoring tools to watch Amazon GameLift, report when something is wrong, and take automatic actions when appropriate:
+ Amazon GameLift Console
+ *Amazon CloudWatch* \-– You can monitor Amazon GameLift metrics in real time, as well as metrics for other AWS resources and applications that you're running on AWS services\. CloudWatch offers a suite of monitoring features, including tools to create customized dashboards and the ability to set alarms that notify or take action when a metric reaches a specified threshold\.
+ *AWS CloudTrail* – captures all API calls and related events made by or on behalf of your AWS account for Amazon GameLift and other AWS services\. Data is delivered as log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\.
+ *Game session logs* – You can output custom server messages for your game sessions to log files that are stored in Amazon S3\.

**Topics**
+ [Monitor GameLift with Amazon CloudWatch](monitoring-cloudwatch.md)
+ [Logging Amazon GameLift API Calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Logging server messages in Amazon GameLift](logging-server-messages.md)