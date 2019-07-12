# IAM Policy Examples for Amazon GameLift<a name="gamelift-iam-policy-examples"></a>

You can use the following examples to create policies and add the appropriate permissions to your IAM users or user groups\.

## Simple Policy for Administrators<a name="iam-policy-simple-example"></a>

This policy provides full administrative access to a user\. Attach it to a user or user group to permit all Amazon GameLift actions on all Amazon GameLift resources \(fleets, aliases, game sessions, player sessions, etc\.\)\.

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

## Simple Policy for Players<a name="iam-policy-admin-game-dev-example"></a>

This policy enables access only to functionality needed by players who are using a game client to connect to an Amazon GameLift\-hosted game server\. This policy allows a user to get game session information, create new game sessions, and join a game session\.

```
{
"Version": "2012-10-17",
"Statement":
  { 
    "Effect": "Allow", 
    "Action": [ "gamelift:CreateGameSession", "gamelift:DescribeGameSessions", "gamelift:SearchGameSessions", "gamelift:CreatePlayerSession" ], 
    "Resource": "*" 
  }
}
```