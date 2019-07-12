# Add FlexMatch to a Game Server<a name="match-server"></a>

This topic describes how to add FlexMatch matchmaking support to your game server\. To learn more about adding FlexMatch to your games, see these topics:
+ [How Amazon GameLift FlexMatch Works](gamelift-match.md)
+ [FlexMatch Integration Roadmap](match-tasks.md)

The information in this topic assumes that you've successfully integrated the Amazon GameLift Server SDK into your game server project, as described in [Add Amazon GameLift to Your Game Server](gamelift-sdk-server-api.md)\. With this work completed, you have most of the mechanisms you need\. The sections in this topic cover the remaining work to handle games that are set up with FlexMatch\.

## Set Up Your Game Server for Matchmaking<a name="match-server-setup"></a>

To set up your game server to handle matched games, complete the following tasks\.

1. **Start game sessions created with matchmaking\.** To request a new game session, Amazon GameLift sends an `onStartGameSession()` request to your game server with a game session object \(see [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\)\. Your game server uses the game session information, including customized game data, to start the requested game session\. For more details, see [Start a Game Session](gamelift-sdk-server-api.md#gamelift-sdk-server-startsession)\. 

   For matched games, the game session object also contains a set of matchmaker data\. Matchmaker data includes information that your game server needs to start a new game session for the match\. This includes the match's team structure, team assignments, and certain player attributes that may be relevant to your game\. For example, your game might unlock certain features or levels based on the average player skill level, or choose a map based on players' preferences\. Learn more in [Work with Matchmaker Data](#match-server-data)\.

1. **Handle player connections\.** When connecting to a matched game, a game client references a player ID and a player session ID \(see [Validate a New Player](gamelift-sdk-server-api.md#gamelift-sdk-server-validateplayer)\)\. Your game server uses the player ID to associate an incoming player with player information in the matchmaker data\. Matchmaker data identifies a player's team assignment and may provide other information to correctly represent the player in the game\. 

1. **Report when players leave a game\.** Make sure that your game server is calling the Server API `RemovePlayerSession()` to report dropped players \(see [Report a Player Session Ending](gamelift-sdk-server-api.md#gamelift-sdk-server-droppedplayer)\)\. This step is important if you're using FlexMatch backfill to fill empty slots in existing games\. It is critical if your game initiates backfill requests through a client\-side game service\. Learn more on implementing FlexMatch backfill in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

1. **Request new players for existing matched game sessions \(optional\)\.** Decide how you want to backfill your existing matched games\. If your matchmakers have backfill mode set to "manual", you may want to add backfill support to your game\. If backfill mode is set to "automatic", you may need a way to turn it off for individual game sessions\. For example, you might want to stop backfilling a game session once a certain point in the game is reached\. Learn more about managing match backfill in [Backfill Existing Games with FlexMatch](match-backfill.md)\. 

## Work with Matchmaker Data<a name="match-server-data"></a>

Your game server must be able to recognize and use the game information in a [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html) object\. The Amazon GameLift service passes these objects to your game server whenever a game session is started or updated\. Core game session information includes game session ID and name, maximum player count, connection information, and custom game data \(if provided\)\.

For game sessions that are created using FlexMatch, the `GameSession` object also contains a set of matchmaker data\. In addition to a unique match ID, it identifies the matchmaker that created the match and describes the teams, team assignments, and players\. It includes the player attributes from the original matchmaking request \(see the [Player](https://docs.aws.amazon.com/gamelift/latest/apireference/API_Player.html) object\)\. It doesn't include the player latency; if you need latency data on current players, such as for match backfill, we recommend getting fresh data\.

**Note**  
Matchmaker data specifies the full matchmaking configuration ARN, which identifies the configuration name, AWS account, and region\. When requesting match backfill from a game client or service, need the configuration name only\. You can extract the configuration name by parsing out the string that follows ":matchmakingconfiguration/"\. In the example shown, the matchmaking configuration name is "MyMatchmakerConfig"\.

The following JSON shows a typical set of matchmaker data\. This example describes a two\-player game, with players matched based on skill ratings and highest level attained\. The matchmaker also matched based on character, and ensured that matched players have at least one map preference in common\. In this scenario, the game server should be able to determine which map is most preferred and use it in the game session\.

```
{
	"matchId":"1111aaaa-22bb-33cc-44dd-5555eeee66ff",
	"matchmakingConfigurationArn":"arn:aws:gamelift:us-west-2:111122223333:matchmakingconfiguration/MyMatchmakerConfig",
	"teams":[
	   {"name":"attacker",
		"players":[
           {"playerId":"4444dddd-55ee-66ff-77aa-8888bbbb99cc",
			"attributes":{
				"skills":{
					"attributeType":"STRING_DOUBLE_MAP",
					"valueAttribute":{"Body":10.0,"Mind":12.0,"Heart":15.0,"Soul":33.0}}
			}
		}]
	},{
		"name":"defender",
		"players":[{
			"playerId":"3333cccc-44dd-55ee-66ff-7777aaaa88bb",
			"attributes":{
				"skills":{
					"attributeType":"STRING_DOUBLE_MAP",
					"valueAttribute":{"Body":11.0,"Mind":12.0,"Heart":11.0,"Soul":40.0}}
			}
		}]
	}]
}
```