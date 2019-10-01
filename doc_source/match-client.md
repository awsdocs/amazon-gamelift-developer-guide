# Add FlexMatch to a Game Client<a name="match-client"></a>

This topic describes how to add FlexMatch matchmaking support to your game clients\. To learn more about FlexMatch and how to set up a custom matchmaker for your games, see these topics:
+ [How Amazon GameLift FlexMatch Works](gamelift-match.md)
+ [Setting Up Amazon GameLift FlexMatch Matchmakers](matchmaker-build.md)
+ [FlexMatch Integration Roadmap](match-tasks.md)

To enable FlexMatch on your game clients, you need to prepare your game client project and then add the following functionality:
+ Request matchmaking for one or multiple players\.
+ Track the status of matchmaking requests\.
+ Request player acceptance for a proposed match\.
+ Once a game session is created for the new match, get player connection information and join the game\.

## Prepare Client Service for Matchmaking<a name="match-client-setup"></a>

We highly recommend that your game client make matchmaking requests through a client\-side game service instead of directly\. By using a trusted source, you can more easily protect against hacking attempts and fake player data\. If your game has a session directory service, this is a good option for handling matchmaking requests\.

To prepare your client service, do the following tasks:
+ **Add the GameLift API\.** Your client service uses functionality in the GameLift API, which is part of the AWS SDK\. See [For Client Services](gamelift-supported.md#gamelift-supported-clients) to learn more about the AWS SDK and download the latest version\. Add this SDK to your game client service project\.
+ **Set up a matchmaking tickets system\.** All matchmaking requests must be assigned a unique ticket ID\. You need a mechanism to generate unique IDs and assign them to new match requests\. A ticket ID can use any string format, up to a maximum of 128 characters\. 
+ **Get matchmaker information\.** Get the name of the matchmaking configuration that you plan to use\. You also need the matchmaker's list of required player attributes, which are defined in the matchmaker's rule set\. 
+ **Get player data\.** Set up a way to get relevant data for each player\. This includes player ID, player attribute values, and updated latency data for each region where the player is likely be slotted into a game\. 
+ **\(optional\) Enable match backfill\.** Decide how you want to backfill your existing matched games\. If your matchmakers have backfill mode set to "manual", you may want to add backfill support to your game\. If backfill mode is set to "automatic", you may need a way to turn it off for individual game sessions\. Learn more about managing match backfill in [Backfill Existing Games with FlexMatch](match-backfill.md)\.

## Request Matchmaking for Players<a name="match-client-start"></a>

Add code to your client service to create and manage matchmaking requests to a FlexMatch matchmaker\. 

**Create a matchmaking request:**
+ Call the GameLift API [StartMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchmaking.html)\. Each request must contain the following information\.  
**Matchmaker**  
The name of the matchmaking configuration to use for the request\. FlexMatch places each request into the pool for the specified matchmaker, and the request is processed based on how the matchmaker is configured\. This includes enforcing a time limit, whether to request player acceptance of matches, which queue to use when placing a resulting game session, etc\. Learn more about matchmakers and rules sets in [Design a FlexMatch Matchmaker](match-configuration.md)\.   
**Ticket ID**  
A unique ticket ID assigned to the request\. Everything related to the request, including events and notifications, will reference the ticket ID\.   
**Player data**  
List of players that you want to create a match for\. If any of the players in the request do not meet match requirements, based on the match rules and latency minimums, the matchmaking request will never result in a successful match\. You can include up to ten players in a match request\. When there are multiple players in a request, FlexMatch tries to create a single match and assign all players to the same team \(randomly selected\)\. If a request contains too many players to fit in one of the match teams, the request will fail to be matched\. For example, if you've set up your matchmaker to create 2v2 matches \(two teams of two players\), you cannot send a matchmaking request containing more than two players\.  
A player \(identified by their player ID\) can only be included in one active matchmaking request at a time\. When you create a new request for a player, any active matchmaking tickets with the same player ID are automatically canceled\.
For each listed player, include the following data:  
  + *Player ID *– Each player must have a unique player ID, which you generate\. See [Generate Player IDs](player-sessions-player-identifiers.md)\. 
  + *Player attributes* – If the matchmaker in use calls for player attributes, the request must provide those attributes for each player\. The required player attributes are defined in the matchmaker's rule set, which also specifies the data type for the attribute\. A player attribute is optional only when the rule set specifies a default value for the attribute\. If the match request does not provide required player attributes for all players, the matchmaking request can never succeed\. Learn more about matchmaker rule sets and player attributes in [Build a FlexMatch Rule Set](match-rulesets.md)\.
  + *Player latencies* – If the matchmaker in use has a player latency rule, the request must report latency for each player\. Player latency data is a list of one or more values per player\. It represents the latency that the player experiences for regions in the matchmaker's queue\. If no latency values for a player are included in the request, the player cannot be matched, and the request fails\.

**Retrieve match request details:**
+ Once a match request is sent, you can view the request details by calling [DescribeMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html) with the request's ticket ID\. This call returns the request information, including current status\. Once a request has been successfully completed, the ticket also contains the information that a game client needs to connect to the match\. 

**Cancel a match request:**
+ You can cancel a matchmaking request at any time by calling [StopMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopMatchmaking.html) with the request's ticket ID\.

## Track Matchmaking Request Status<a name="match-client-track"></a>

Add code to your client service to track the status of all matchmaking requests and respond as needed\. There are a couple of options available for tracking status\.

**Event notifications**  
Set up notifications to track events that GameLift emits for matchmaking processes\. This is the recommended method; it is simple to set up and an efficient use of resources\. You can set up notifications either directly, by creating an SNS topic, or by using CloudWatch Events\. For more information on setting up notifications, see [Set up FlexMatch Event Notification](match-notification.md)\. Once you've set up notifications, add a listener on your client service to detect the events and respond as needed\. It's also a good idea to poll for status updates whenever 30 seconds has passed with no notification\. 

**Continuous polling**  
Retrieve a matchmaking request ticket, including current status, by calling [DescribeMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html) with the request's ticket ID\. We recommend polling no more than once every 10 seconds\.

## Request Player Acceptance<a name="match-client-accept"></a>

If you're using a matchmaker that has player acceptance turned on, add code to your client service to manage the player acceptance process\.

**Request player acceptance for a proposed match:**

1. **Detect when a proposed match needs player acceptance\.** Monitor the matchmaking ticket to detect when the status changes to `REQUIRES_ACCEPTANCE`\. If you're monitoring notifications, a change to this status triggers the FlexMatch event `MatchmakingRequiresAcceptance`\.

1. **Get acceptances from all players\.** Create a mechanism to present the proposed match details to every player in the matchmaking ticket\. Players must be able to indicate that they either accept or reject the proposed match\. You can retrieve match details by calling [DescribeMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html)\. Players have a limited time to respond before the matchmaker withdraws the proposed match and moves on\.

1. **Report player responses to FlexMatch\.** Report player responses by calling [AcceptMatch](https://docs.aws.amazon.com/gamelift/latest/apireference/API_AcceptMatch.html) with either accept or reject\. All players in a matchmaking request must accept the match for it to go forward\.

1. **Handle tickets with failed acceptances\.** A request fails when any player in the proposed match either rejects the match or fails to respond by the acceptance time limit\. 

## Connect to a Match<a name="match-client-connect"></a>

Add code to your client service to handle a successfully completed match \(status `COMPLETED` or event `MatchmakingSucceeded`\)\. This includes notifying the match's players and handing off connection information to their game clients\. 

When a matchmaking request is completed, connection information is added to the matchmaking ticket\. To retrieve a completed matchmaking ticket by calling [DescribeMatchmaking](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeMatchmaking.html)\. Connection information includes the game session's IP address and port, as well as a player session ID for each player ID\. Learn more in [GameSessionConnectionInfo](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionConnectionInfo.html)\. 

Your game client uses this information to connect directly to the game session that is hosting the match\. A connection request for a matched game session should include a player session ID and a player ID\. This data associates the connected player to the game session's match data, which includes team assignments \(see [GameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSession.html)\)\. 

## Sample StartMatchmaking Requests<a name="match-client-sample"></a>

These code snippets build matchmaking requests for several different matchmakers\. As described, a request must provide the player attributes that are required by the matchmaker in use, as defined in the matchmaker's rule set\. The attribute provided must use the same data type, number \(N\) or string \(S\) that is defined in the rule set\. 

```
# Uses matchmaker for two-team game mode based on player skill level
def start_matchmaking_for_cowboys_vs_aliens(config_name, ticket_id, player_id, skill, team):
    response = gamelift.start_matchmaking(
        ConfigurationName=config_name,
        Players=[{
            "PlayerAttributes": {
                "skill": {"N": skill}
            },
            "PlayerId": player_id,
            "Team": team
        }],
        TicketId=ticket_id)

# Uses matchmaker for monster hunter game mode based on player skill level
def start_matchmaking_for_players_vs_monster(config_name, ticket_id, player_id, skill, is_monster):
    response = gamelift.start_matchmaking(
        ConfigurationName=config_name,
        Players=[{
            "PlayerAttributes": {
                "skill": {"N": skill},
                "desiredSkillOfMonster": {"N": skill},
                "wantsToBeMonster": {"N": int(is_monster)}
            },
            "PlayerId": player_id
        }],
        TicketId=ticket_id)

# Uses matchmaker for brawler game mode with latency
def start_matchmaking_for_three_team_brawler(config_name, ticket_id, player_id, skill, role):
    response = gamelift.start_matchmaking(
        ConfigurationName=config_name,
        Players=[{
            "PlayerAttributes": {
                "skill": {"N": skill},
                "character": {"S": [role]},
            },
            "PlayerId": player_id,
            "LatencyInMs": { "us-west-2": 20}
        }],
        TicketId=ticket_id)

# Uses matchmaker for multiple game modes and maps based on player experience
def start_matchmaking_for_multi_map(config_name, ticket_id, player_id, skill, maps, modes):
    response = gamelift.start_matchmaking(
        ConfigurationName=config_name,
        Players=[{
            "PlayerAttributes": {
                "experience": {"N": skill},
                "gameMode": {"SL": modes},
                "mapPreference": {"SL": maps}
            },
            "PlayerId": player_id
        }],
        TicketId=ticket_id)
```