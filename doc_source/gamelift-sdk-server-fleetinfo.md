# Get fleet data for a GameLift instance<a name="gamelift-sdk-server-fleetinfo"></a>

There are some situations where your custom game build or Realtime Servers script may require information about the GameLift fleet\. For example, your game build or script might include code to:
+ Monitor activity based on fleet data\.
+ Roll up metrics to track activity by fleet data\. \(Many games use this data for LiveOps activities\.\)
+ Provide relevant data to custom game services, such as for matchmaking, additional capacity scaling, or testing\.

Fleet information is available as a JSON file on each instance in the following locations:
+ Windows: `C:\GameMetadata\gamelift-metadata.json`
+ Linux: `/local/gamemetadata/gamelift-metadata.json`

The `gamelift-metadata.json` file includes the [attributes of a GameLift fleet resource](https://docs.aws.amazon.com/gamelift/latest/apireference/API_FleetAttributes.html)\.

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