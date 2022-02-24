# IAM policy examples for GameLift<a name="gamelift-iam-policy-examples"></a>

You can use the following examples to create inline policies and add the appropriate permissions to your AWS Identity and Access Management \(IAM\) users or user groups\.

## Simple policy examples for administrators<a name="iam-policy-simple-example"></a>

These IAM policy examples illustrate how to provide full administrative access to a user\.

**Policy for GameLift resource permissions**  
The following policy example covers access to all GameLift\-related resources \(fleets, queues, game sessions, matchmakers, etc\.\)\. All users who manage or view these resources need this type of permissions policy\.

```
{
"Version": "2012-10-17",
"Statement":
  { 
    "Effect": "Allow", 
    "Action": "gamelift:*", 
    "Resource": "*" 
  }
}
```

**Policy for GameLift resource permissions with opt\-in AWS Region support**  
If you choose to use a [multi\-location fleet](gamelift-regions.md#gamelift-regions-hosting) with instances in opt\-in Regions, you must enable the Region for your account\. Also, your GameLift administrator policy must allow the `ec2:DescribeRegions` action\. For more information about opt\-in Regions and how to enable them, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\. The following policy includes support for opt\-in Regions\.

```
{
"Version": "2012-10-17",
"Statement":
  { 
    "Effect": "Allow", 
    "Action": [
      "ec2:DescribeRegions",
      "gamelift:*"
    ], 
    "Resource": "*" 
  }
}
```

**Policy for GameLift resource and PassRole permissions**  
This policy example provides access to GameLift\-related resources as above\. It also allows the user to pass an IAM service role to GameLift\. Not all users need the PassRole permission; this permission is used to give GameLift limited ability to access resources in other services on your behalf\. For example, you need this permission when calling `CreateBuild` with an IAM role that allows GameLift to access your build files in an Amazon Simple Storage Service \(Amazon S3\) bucket\. For more information on PassRole, see [IAM: Pass an IAM role to a specific AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_examples_iam-passrole-service.html) in the *IAM User Guide*\.

```
{
"Version": "2012-10-17",
"Statement":[
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
        "StringEquals": {"iam:PassedToService": "gamelift.amazonaws.com"}
      }
  }]
}
```

## Simple policy examples for players<a name="iam-policy-admin-game-dev-example"></a>

The following IAM policy examples illustrate how to enable game clients and/or game client services with the functionality to get players into game sessions\. These examples cover the key scenarios that games might use to start new game sessions and assign players to available player slots\.

**Policy for game session placements**  
This policy example is for a game client service that uses game session queues and placements to start new game sessions\. Players might be added to a game session either in the initial placement request or by creating new player sessions for an existing game session\.

```
{
"Version": "2012-10-17",
"Statement":
  { 
    "SID": "PlayerPermissionsForGameSessionPlacements",
    "Effect": "Allow", 
    "Action": [ 
          "gamelift:StartGameSessionPlacement", 
          "gamelift:DescribeGameSessionPlacement", 
          "gamelift:StopGameSessionPlacement", 
          "gamelift:CreatePlayerSession", 
          "gamelift:CreatePlayerSessions", 
          "gamelift:DescribeGameSessions" ], 
    "Resource": "*" 
  }
}
```

**Policy for matchmaking**  
This policy example is for a game client or client service that uses GameLift FlexMatch matchmaking\. Players might be matched and placed into a new game session or they might be added to an existing game session through the backfill process\. 

```
{
"Version": "2012-10-17",
"Statement":
  { 
    "SID": "PlayerPermissionsForGameSessionMatchmaking",
    "Effect": "Allow", 
    "Action": [ 
          "gamelift:StartMatchmaking", 
          "gamelift:DescribeMatchmaking", 
          "gamelift:StopMatchmaking", 
          "gamelift:AcceptMatch", 
          "gamelift:StartMatchBackfill", 
          "gamelift:DescribeGameSessions" ], 
    "Resource": "*" 
  }
}
```

**Policy for manual game session placement**  
This policy example is for a game client or client service that creates new game sessions on specific fleets and might create new player sessions in specific game sessions\. This scenario supports a game that uses the "list\-and\-pick" method to let players choose from list of available game sessions\.

```
{
"Version": "2012-10-17",
"Statement":
  { 
    "SID": "PlayerPermissionsForManualGameSessions",
    "Effect": "Allow", 
    "Action": [ 
          "gamelift:CreateGameSession", 
          "gamelift:DescribeGameSessions", 
          "gamelift:SearchGameSessions", 
          "gamelift:CreatePlayerSession", 
          "gamelift:CreatePlayerSessions", 
          "gamelift:DescribePlayerSessions" ], 
    "Resource": "*" 
  }
}
```