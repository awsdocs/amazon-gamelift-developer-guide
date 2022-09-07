# Infrastructure security in<a name="infrastructure-security"></a>

If you're using GameLift FleetIQ as a standalone feature with Amazon EC2, also refer to [Security in Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html) in the *Amazon EC2 User Guide for Linux Instances*\.

As a managed service, is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of security processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

The service places all fleets into Amazon virtual private clouds \(VPCs\) so that each fleet exists in a logically isolated area in the AWS Cloud\. You can use policies to control access from specific VPC endpoints or specific VPCs\. Effectively, this isolates network access to a given resource from only the specific VPC within the AWS network\. When you create a fleet, you specify a range of port numbers and IP addresses\. These ranges limit how inbound traffic can access hosted game servers on a fleet VPC\. Use standard security best practices when choosing fleet access settings\.