# Prepare Your Game Client in Amazon Lumberyard<a name="game-client-intro"></a>

All game clients must be configured to enable communication with the Amazon GameLift service, including specifics on which fleet to use, access credentials, how to connect, etc\. The simplest method is to create a batch file that sets the console variables listed as follows\.

**Tip**  
[Learn more about ways to explore Amazon GameLift features, including Realtime Servers, using sample games](gamelift-explore.md)\.

**To prepare the game client**

1. In your batch file, set the following console variables to launch the game client\. These variables have been added to `\dev\Code\CryEngine\CryNetwork\Lobby\LobbyCvars`
   + `gamelift_aws_access_key` = part of the IAM [security credentials](setting-up-aws-login.md) for a user with "player" access in your AWS account 
   + `gamelift_aws_secret_key` = part of the IAM [security credentials](setting-up-aws-login.md) for a user with "player" access in your AWS account
   + `gamelift_fleet_id` = Unique ID of an active fleet to connect to
   + `gamelift_alias_id` = Unique ID of an alias pointing to a fleet to connect to
   + \(Optional\) `gamelift_endpoint` = Amazon GameLift server endpoint; the default value is `gamelift.us-west-2.amazonaws.com`
   + \(Optional\) `gamelift_aws_region` = AWS region name; default value is `us-west-2`
   + \(Optional\) `gamelift_player_id` = ID that you generate to [uniquely identify a player](player-sessions-player-identifiers.md)

1. Add the following command to launch the server browser: 

   Follow this pattern when using an Amazon GameLift fleet ID \(`gamelift_fleet_id`\):

   ```
   .\Bin64\[your game executable] +gamelift_fleet_id [your fleet ID] +gamelift_aws_region us-west-2 +gamelift_aws_access_key [your AWS access key] +gamelift_aws_secret_key [your AWS secret key] +sv_port 64091 +map [map name]
   ```

   Follow this pattern when using an Amazon GameLift alias ID \(`gamelift_alias_id`\):

   ```
   .\Bin64\[your game executable] +gamelift_alias_id  [your alias ID] +gamelift_aws_region us-west-2 +gamelift_aws_access_key [your AWS access key] +gamelift_aws_secret_key [your AWS secret key] +sv_port 64091 +map [map name]
   ```