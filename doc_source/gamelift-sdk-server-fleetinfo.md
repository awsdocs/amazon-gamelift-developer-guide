# Get fleet data for a GameLift instance<a name="gamelift-sdk-server-fleetinfo"></a>

There are some situations where your custom game build or Realtime Servers script, once deployed and running on a GameLift instance, may need to access information about the fleet that the instance belongs to\. For example, your game build or script might include code to: 
+ Monitor instance activity filtered based on fleet data\. 
+ Roll up instance metrics to track activity by fleet data\. Many games use this data for live\-ops activities\.
+ Provide relevant data to custom game services, such as for matchmaking, additional capacity scaling, or A/B testing\.

Fleet information is available as a JSON file on each instance in the following locations:
+ Windows: `C:\GameMetadata\gamelift-metadata.json`
+ Linux: `/local/gamemetadata/gamelift-metadata.json`

The `gamelift-metadata.json` file includes the following fleet data, which corresponds to the attributes of a [GameLift Fleet resource](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetAttributes.html)\.
+ `fleetArn` – [Amazon resource name](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) \(ARN\) that identifies the fleet of the current instance\. This identifier is unique across all AWS Regions\.
+ `fleetId` – Unique identifier that is assigned to the fleet of the current instance\. This identifier is specific to an AWS Region\.
+ `fleetName` – Descriptive label given to the fleet of the current instance\.
+ `fleetDescription` – Human\-readable description of the fleet\. A fleet does not require a description attribute\.
+ `fleetType` – Indicates whether the fleet uses Spot Instances or On\-Demand instances\. The fleet type determines whether all instances in a fleet Spot instances only or On\-Demand Instances only\.
+ `serverLaunchPath` – The location of a custom game build executable or a Realtime script file on the instance\. Every fleet has a runtime configuration with a list of server processes to launch and run on every instance in the fleet\. The server process configuration includes the launch path to the executable or script file\. See more about a fleet's [RuntimeConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_RuntimeConfiguration.html) and [ServerProcess](https://docs.aws.amazon.com/gamelift/latest/apireference/API_ServerProcess.html)\.
+ `instanceType` – EC2 instance type of the current instance\. All instances in the fleet use the same instance type\. See [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) for detailed descriptions\.
+ `instanceRoleArn` – AWS Identity and Access Management role with permissions that allows the current instance to interact with other AWS resources that you own\. If a fleet is configured with an IAM role, the role is available to all instances in the fleet\. With an IAM role, any application in your deployed game build or script, including install scripts, server processes, and daemons, can access the AWS resources as permitted in the role\.
+ `buildArn` – [Amazon resource name](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) \(ARN\) for the custom game build that is deployed to the current instance\. This identifier is unique across all AWS Regions\. An instance is deployed with either a custom game build or Realtime Servers script\.
+ `buildId` – Unique identifier assigned to the custom game build that is deployed to the current instance\.
+ `scriptArn` – [Amazon resource name](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) \(ARN\) for the Realtime Servers script that is deployed to the current instance\. This identifier is unique across all AWS Regions\. An instance is deployed with either a custom game build or Realtime Servers script\.
+ `scriptId` – Unique identifier assigned to the Realtime Servers script that is deployed to the current instance\.

Example JSON file:

```
{
    "buildArn":"arn:aws:gamelift:us-west-2:123456789012:build/build-1111aaaa-22bb-33cc-44dd-5555eeee66ff",
    "buildId":"build-1111aaaa-22bb-33cc-44dd-5555eeee66ff",
    "fleetArn":"arn:aws:gamelift:us-west-2:123456789012:fleet/fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa",
    "fleetDescription":"Test fleet for Really Fun Game v0.8",
    "fleetId":"fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa",
    "fleetName":"ReallyFunGameTestFleet08",
    "fleetType":"ON_DEMAND",
    "instanceRoleArn":"arn:aws:iam::123456789012:role/S3AccessForGameLift",
    "instanceType":"c5.large",
    "serverLaunchPath":"/local/game/reallyfungame.exe"
}
```