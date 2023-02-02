# Data protection in GameLift<a name="data-protection"></a>

If you're using GameLift FleetIQ as a standalone feature with Amazon EC2, also refer to [Security in Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html) in the *Amazon EC2 User Guide for Linux Instances*\.

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in Amazon GameLift\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual users with AWS IAM Identity Center \(successor to AWS Single Sign\-On\) or AWS Identity and Access Management \(IAM\)\. That way, each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing sensitive data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form fields such as a **Name** field\. This includes when you work with GameLift or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.

GameLift\-specific data is handled as follows:
+ Game server builds and scripts that you upload to GameLift are stored in Amazon S3\. There is no direct customer access to this data once it is uploaded\. An authorized user can get temporary access to upload files, but can't view or update the files in Amazon S3 directly\. To delete scripts and builds, use the GameLift console or the service API\.
+ Game session log data is stored in Amazon S3 for a limited period of time after the game session is completed\. Authorized users can access the log data by downloading it via a link in the GameLift console or by calls to the service API\. 
+ Metric and event data is stored in GameLift and can be accessed through the GameLift console or by calls to the service API\. Data can be retrieved on fleets, instances, game session placements, matchmaking tickets, game sessions, and player sessions\. Data can also be accessed through Amazon CloudWatch and CloudWatch Events\.
+ Customer\-supplied data is stored in GameLift \. Authorized users can access it by calls to the service API\. Potentially sensitive data might include player data, player session and game session data \(including connection info\), matchmaker data, and so on\. 
**Note**  
If you provide custom player IDs in your requests, it is expected that these values are anonymized UUIDs and contain no identifying player information\.

For more information about data protection, see the [AWS shared responsibility model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

## Encryption at rest<a name="encryption-at-rest"></a>

At\-rest encryption of GameLift\-specific data is handled as follows:
+ Game server builds and scripts are stored in Amazon S3 buckets with server\-side encryption\.
+ Customer\-supplied data is stored in GameLift in an encrypted format\.

## Encryption in transit<a name="encryption-in-transit"></a>

Connections to the GameLift APIs are made over a secure \(SSL\) connection and authenticated using [AWS Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) \(when connecting through the AWS CLI or AWS SDK, signing is handled automatically\)\. Authentication is managed using the IAM\-defined access policies for the security credentials that are used to make the connection\.

Direct communication between game clients and game servers is as follows: 
+ For custom game servers being hosted on GameLift resources, communication does not involve the GameLift service\. Encryption of this communication is the responsibility of the customer\. You can use TLS\-enabled fleets to have your game clients authenticate the game server on connection and to encrypt all communication between your game client and game server\.
+ For Realtime Servers with TLS certificate generation enabled, traffic between game client and Realtime servers using the Realtime Client SDK is encrypted in flight\. TCP traffic is encrypted using TLS 1\.2, and UDP traffic is encrypted using DTLS 1\.2\.

## Internetwork traffic privacy<a name="inter-network-traffic-privacy"></a>

You can remotely access your GameLift instances securely\. For instances that use Linux, SSH provides a secure communications channel for remote access\. For instances that are running Windows, use a remote desktop protocol \(RDP\) client\. With GameLift FleetIQ, remote access to your instances using AWS Systems Manager Session Manager and Run Command is encrypted using TLS 1\.2, and requests to create a connection are signed using SigV4\. For help with connecting to a managed GameLift instance, see [Remotely access GameLift fleet instances](fleets-remote-access.md)\.