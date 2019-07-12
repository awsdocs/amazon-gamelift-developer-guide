# Logging Amazon GameLift API Calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon GameLift is integrated with AWS CloudTrail, a service that captures all of the API calls made by or on behalf of Amazon GameLift in your AWS account\. CloudTrail delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls from the Amazon GameLift console or from the Amazon GameLift API\. Using the information collected by CloudTrail, you can determine what request was made to Amazon GameLift, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the [https://docs.aws.amazon.com/awscloudtrail/latest/userguide/](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon GameLift Information in CloudTrail<a name="service-name-info-cloudtrail"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to Amazon GameLift actions are tracked in log files\. Amazon GameLift records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\.

All Amazon GameLift actions are logged by CloudTrail\. For example, calls to `CreateGameSession`, `CreatePlayerSession` and `UpdateGameSession` generate entries in the CloudTrail log files\. For the complete list of actions, see the *[Amazon GameLift API Reference](https://docs.aws.amazon.com/gamelift/latest/apireference/)*\.

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with AWS account root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the `userIdentity` field in the [CloudTrail Event Reference](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your S3 bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. 

If you want to take quick action upon log file delivery, you can choose to have CloudTrail publish Amazon Simple Notification Service \(Amazon SNS\) notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)\.

You can also aggregate Amazon GameLift log files from multiple AWS regions and multiple AWS accounts into a single S3 bucket\. For more information, see [Aggregating CloudTrail Log Files to a Single Amazon S3 Bucket](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html)\.

## Understanding Amazon GameLift Log File Entries<a name="understanding-service-name-entries"></a>

CloudTrail log files can contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order\. That is, they are not an ordered stack trace of the public API calls\.

The following example shows a CloudTrail log entry that demonstrates the `CreateFleet` and `DescribeFleetAttributes` actions\.

```
{
    "Records": [
        {
            "eventVersion": "1.04",
            "userIdentity": {
                "type": "IAMUser",
                "principalId": "AIDACKCEVSQ6C2EXAMPLE",
                "arn": "arn:aws:iam::111122223333:user/myUserName",
                "accountId": "111122223333",
                "accessKeyId": AKIAIOSFODNN7EXAMPLE",
                "userName": "myUserName"
            },
            "eventTime": "2015-12-29T23:40:15Z",
            "eventSource": "gamelift.amazonaws.com",
            "eventName": "CreateFleet",
            "awsRegion": "us-west-2",
            "sourceIPAddress": "192.0.2.0",
            "userAgent": "[]",
            "requestParameters": {
                "buildId": "build-92b6e8af-37a2-4c10-93bd-4698ea23de8d",
                "eC2InboundPermissions": [
                    {
                        "ipRange": "10.24.34.0/23",
                        "fromPort": 1935,
                        "protocol": "TCP",
                        "toPort": 1935
                    }
                ],
                "logPaths": [
                    "C:\\game\\serverErr.log",
                    "C:\\game\\serverOut.log"
                ],
                "eC2InstanceType": "c4.large",
                "serverLaunchPath": "C:\\game\\MyServer.exe",
                "description": "Test fleet",
                "serverLaunchParameters": "-paramX=baz",
                "name": "My_Test_Server_Fleet"
            },
            "responseElements": {
                "fleetAttributes": {
                    "fleetId": "fleet-0bb84136-4f69-4bb2-bfec-a9b9a7c3d52e",
                    "serverLaunchPath": "C:\\game\\MyServer.exe",
                    "status": "NEW",
                    "logPaths": [
                        "C:\\game\\serverErr.log",
                        "C:\\game\\serverOut.log"
                    ],
                    "description": "Test fleet",
                    "serverLaunchParameters": "-paramX=baz",
                    "creationTime": "Dec 29, 2015 11:40:14 PM",
                    "name": "My_Test_Server_Fleet",
                    "buildId": "build-92b6e8af-37a2-4c10-93bd-4698ea23de8d"
                }
            },
            "requestID": "824a2a4b-ae85-11e5-a8d6-61d5cafb25f2",
            "eventID": "c8fbea01-fbf9-4c4e-a0fe-ad7dc205ce11",
            "eventType": "AwsApiCall",
            "recipientAccountId": "111122223333"
        },
        {
            "eventVersion": "1.04",
            "userIdentity": {
                "type": "IAMUser",
                "principalId": "AIDACKCEVSQ6C2EXAMPLE",
                "arn": "arn:aws:iam::111122223333:user/myUserName",
                "accountId": "111122223333",
                "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
                "userName": "myUserName"
            },
            "eventTime": "2015-12-29T23:40:15Z",
            "eventSource": "gamelift.amazonaws.com",
            "eventName": "DescribeFleetAttributes",
            "awsRegion": "us-west-2",
            "sourceIPAddress": "192.0.2.0",
            "userAgent": "[]",
            "requestParameters": {
                "fleetIds": [
                    "fleet-0bb84136-4f69-4bb2-bfec-a9b9a7c3d52e"
                ]
            },
            "responseElements": null,
            "requestID": "82e7f0ec-ae85-11e5-a8d6-61d5cafb25f2",
            "eventID": "11daabcb-0094-49f2-8b3d-3a63c8bad86f",
            "eventType": "AwsApiCall",
            "recipientAccountId": "111122223333"
        },
    ]
}
```