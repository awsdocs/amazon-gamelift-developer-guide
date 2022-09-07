# IAM policy examples for GameLift<a name="gamelift-iam-policy-examples"></a>

Use the following examples to create inline AWS Identity and Access Management \(IAM\) policies and add required permissions to your IAM users or user groups\.

If you're using GameLift FleetIQ as a standalone solution, see [Set up your AWS account for GameLift FleetIQ](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-iam-permissions.html)\.

## Administrator policy examples<a name="iam-policy-simple-example"></a>

These IAM policy examples provide a user with full GameLift administrative access\.

**Example inline policy for GameLift resource permissions**  
The following policy example provides access to all GameLift resources\. All users who manage or view these resources need this type of permissions policy\.  

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "gamelift:*",
    "Resource": "*"
  }
}
```

**Example inline policy for GameLift resource permissions with support for Regions that aren't enabled by default**  
The following policy example provides access to all GameLift resources and AWS Regions that aren't enabled by default\. For more information about Regions that aren't enabled by default and how to enable them, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\.  

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": [
      "ec2:DescribeRegions",
      "gamelift:*"
    ],
    "Resource": "*"
  }
}
```

**Example inline policy for GameLift resource and `PassRole` permissions**  
The following policy example provides access to all GameLift resources and allows the user to pass an IAM service role to GameLift\. Use this permission to give GameLift limited ability to access resources in other services on your behalf\. For example, this permission allows GameLift to access your build files in Amazon S3 when calling `CreateBuild`\. For more information about the `PassRole` action, see [IAM: Pass an IAM role to a specific AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_examples_iam-passrole-service.html) in the *IAM User Guide*\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "gamelift:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "gamelift.amazonaws.com"
        }
      }
    }
  ]
}
```

## Player policy examples<a name="iam-policy-admin-game-dev-example"></a>

These IAM policy examples provide game clients and game client services with the functionality to put players into game sessions\. The examples cover the key scenarios that games might use to start new game sessions and to assign players to available player slots\.

**Example inline policy for game session placement**  
The following policy example is for a game client service that uses game session queues and placements to start new game sessions\. You can add players to a game session in the initial placement request or by creating new player sessions for an existing game session\.  

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "PlayerPermissionsForGameSessionPlacements",
    "Effect": "Allow",
    "Action": [
      "gamelift:StartGameSessionPlacement",
      "gamelift:DescribeGameSessionPlacement",
      "gamelift:StopGameSessionPlacement",
      "gamelift:CreatePlayerSession",
      "gamelift:CreatePlayerSessions",
      "gamelift:DescribeGameSessions"
    ],
    "Resource": "*"
  }
}
```

**Example inline policy for matchmaking**  
The following policy example is for a game client service that uses GameLift FlexMatch matchmaking\. FlexMatch can match players and place them into a new game session, or add them to an existing game session through the backfill process\. For more information about FlexMatch, see [What is Amazon GameLift FlexMatch?](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html)  

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "PlayerPermissionsForGameSessionMatchmaking",
    "Effect": "Allow",
    "Action": [
      "gamelift:StartMatchmaking",
      "gamelift:DescribeMatchmaking",
      "gamelift:StopMatchmaking",
      "gamelift:AcceptMatch",
      "gamelift:StartMatchBackfill",
      "gamelift:DescribeGameSessions"
    ],
    "Resource": "*"
  }
}
```

**Example inline policy for manual game session placement**  
The following policy example allows a game client service to create game sessions on fleets and player sessions in game sessions\. This scenario supports a game that uses the *list\-and\-pick* method to let players choose from list of available game sessions\.  

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Sid": "PlayerPermissionsForManualGameSessions",
    "Effect": "Allow",
    "Action": [
      "gamelift:CreateGameSession",
      "gamelift:DescribeGameSessions",
      "gamelift:SearchGameSessions",
      "gamelift:CreatePlayerSession",
      "gamelift:CreatePlayerSessions",
      "gamelift:DescribePlayerSessions"
    ],
    "Resource": "*"
  }
}
```