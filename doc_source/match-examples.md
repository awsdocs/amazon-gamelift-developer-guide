# FlexMatch Rule Set Examples<a name="match-examples"></a>

FlexMatch rule sets can cover a variety of matchmaking scenarios\. The following examples conform to the FlexMatch configuration structure and property expression language\. Copy these rule sets in their entirety or choose components as needed\.

For more information on using FlexMatch rules and rule sets, see the following topics:
+ [Build a FlexMatch Rule Set](match-rulesets.md)
+ [FlexMatch Rules Language](match-rules-reference.md)

**Note**  
When evaluating a matchmaking ticket that includes multiple players, all players in the request must meet the match requirements\.

## Example 1: Create Two Teams with Evenly Matched Players<a name="match-examples-1"></a>

This example illustrates how to set up two equally matched teams of players with the following instructions\. 
+ Create two teams of players\.
  + Include between four and eight players in each team\.
  + Final teams must have the same number of players\.
+ Include a player’s skill level \(if not provided, default to 10\)\.
+ Choose players based on whether their skill level is similar to other players\. Ensure that both teams have an average player skill within 10 points of each other\.
+ If the match is not filled quickly, relax the player skill requirement to complete a match in reasonable time\. 
  + After 5 seconds, expand the search to allow teams with average player skills within 50 points\. 
  + After 15 seconds, expand the search to allow teams with average player skills within 100 points\. 

Notes on using this rule set: 
+ This example allows for teams to be any size between four and eight players \(although they must be the same size\)\. For teams with a range of valid sizes, the matchmaker makes a best\-effort attempt to match the maximum number of allowed players\.
+ The `FairTeamSkill` rule ensures that teams are evenly matched based on player skill\. To evaluate this rule for each new prospective player, FlexMatch tentatively adds the player to a team and calculates the averages\. If rule fails, the prospective player is not added to the match\.
+ Since both teams have identical structures, you could opt to create just one team definition and set the team quantity to "2"\. In this scenario, if you named the team "aliens", then your teams would be assigned the names "aliens\_1" and "aliens\_2"\.

```
{
    "name": "aliens_vs_cowboys",
    "ruleLanguageVersion": "1.0",
    "playerAttributes": [{
        "name": "skill",
        "type": "number",
        "default": 10
    }],
    "teams": [{
        "name": "cowboys",
        "maxPlayers": 8,
        "minPlayers": 4
    }, {
        "name": "aliens",
        "maxPlayers": 8,
        "minPlayers": 4
    }],
    "rules": [{
        "name": "FairTeamSkill",
        "description": "The average skill of players in each team is within 10 points from the average skill of all players in the match",
        "type": "distance",
        // get skill values for players in each team and average separately to produce list of two numbers
        "measurements": [ "avg(teams[*].players.attributes[skill])" ],
        // get skill values for players in each team, flatten into a single list, and average to produce an overall average
        "referenceValue": "avg(flatten(teams[*].players.attributes[skill]))",
        "maxDistance": 10 // minDistance would achieve the opposite result
    }, {
        "name": "EqualTeamSizes",
        "description": "Only launch a game when the number of players in each team matches, e.g. 4v4, 5v5, 6v6, 7v7, 8v8",
        "type": "comparison",
        "measurements": [ "count(teams[cowboys].players)" ],
        "referenceValue": "count(teams[aliens].players)",
        "operation": "=" // other operations: !=, <, <=, >, >=
    }],
    "expansions": [{
        "target": "rules[FairTeamSkill].maxDistance",
        "steps": [{
            "waitTimeSeconds": 5,
            "value": 50
        }, {
            "waitTimeSeconds": 15,
            "value": 100
        }]
    }]
}
```

## Example 2: Create Uneven Teams \(Hunters vs\. Monster\)<a name="match-examples-2"></a>

This example describes a game mode in which a group of players hunt a single monster\. People choose either a hunter or a monster role\. Hunters specify the minimum skill level for the monster that they want to face\. The minimum size of the hunter team can be relaxed over time to complete the match\. This scenario sets out the following instructions: 
+ Create one team of exactly five hunters\. 
+ Create a separate team of exactly one monster\. 
+ Include the following player attributes:
  + A player’s skill level \(if not provided, default to 10\)\.
  + A player’s preferred monster skill level \(if not provided, default to 10\)\.
  + Whether the player wants to be the monster \(if not provided, default to 0 or false\)\.
+ Choose a player to be the monster based on the following criteria:
  + Player must request the monster role\.
  + Player must meet or exceed the highest skill level preferred by the players who are already added to the hunter team\. 
+ Choose players for the hunter team based on the following criteria:
  + Players who request a monster role cannot join the hunter team\.
  + If the monster role is already filled, player must want a monster skill level that is lower than the skill of the proposed monster\. 
+ If a match is not filled quickly, relax the hunter team's minimum size as follows:
  + After 30 seconds, allow a game to start with only four players in the hunter team\.
  + After 60 seconds, allow a game to start with only three people in the hunter team\.

Notes on using this rule set: 
+ By using two separate teams for hunters and monster, you can evaluate membership based on different sets of criteria\.

```
{
    "name": "players_vs_monster_5_vs_1",
    "ruleLanguageVersion": "1.0",
    "playerAttributes": [{
        "name": "skill",
        "type": "number",
        "default": 10
    },{
        "name": "desiredSkillOfMonster",
        "type": "number",
        "default": 10
    },{
        "name": "wantsToBeMonster",
        "type": "number",
        "default": 0
    }],
    "teams": [{
        "name": "players",
        "maxPlayers": 5,
        "minPlayers": 5
    }, {
        "name": "monster",
        "maxPlayers": 1,
        "minPlayers": 1 
    }],
    "rules": [{
        "name": "MonsterSelection",
        "description": "Only users that request playing as monster are assigned to the monster team",
        "type": "comparison",
        "measurements": ["teams[monster].players.attributes[wantsToBeMonster]"],
        "referenceValue": 1, 
        "operation": "="
    },{
        "name": "PlayerSelection",
        "description": "Do not place people who want to be monsters in the players team",
        "type": "comparison",
        "measurements": ["teams[players].players.attributes[wantsToBeMonster]"],
        "referenceValue": 0,
        "operation": "="
    },{
        "name": "MonsterSkill",
        "description": "Monsters must meet the skill requested by all players",
        "type": "comparison",
        "measurements": ["avg(teams[monster].players.attributes[skill])"],
        "referenceValue": "max(teams[players].players.attributes[desiredSkillOfMonster])",
        "operation": ">="
    }],
    "expansions": [{
        "target": "teams[players].minPlayers",
        "steps": [{
            "waitTimeSeconds": 30,
            "value": 4 
        },{
            "waitTimeSeconds": 60,
            "value": 3 
        }]
    }]
}
```

## Example 3: Set Team\-Level Requirements and Latency Limits<a name="match-examples-3"></a>

This example illustrates how to set up player teams and apply a set of rules to each team instead of each individual player\. It uses a single definition to create three equaIly matched teams\. It also establishes a maximum latency for all players\. Latency maximums can be relaxed over time to complete the match\. This scenario sets out the following instructions:
+ Create three teams of players\.
  + Include between three and five players in each team\.
  + Final teams must contain the same or nearly the same number of players \(within one\)\.
+ Include the following player attributes:
  + A player’s skill level \(if not provided, default to 10\)\.
  + A player’s character role \(if not provided, default to “peasant”\)\.
+ Choose players based on whether their skill level is similar to other players in the match\.
  + Ensure that each team has an average player skill within 10 points of each other\. 
+ Limit teams to the following number of “medic” characters:
  + An entire match can have a maximum of five medics\.
+ Only match players who report latency of 50 milliseconds or less\.
+ If a match is not filled quickly, relax the player latency requirement as follows: 
  + After 10 seconds, allow player latency values up to 100 ms\.
  + After 20 seconds, allow player latency values up to 150 ms\. 

Notes on using this rule set: 
+ The rule set ensures that teams are evenly matched based on player skill\. To evaluate the `FairTeamSkill` rule, FlexMatch tentatively adds the prospective player to a team and calculates the average skill of players in the team\. It then compares it against the average skill of players in both teams\. If rule fails, the prospective player is not added to the match\.
+ The team\- and match\-level requirements \(total number of medics\) are achieved through a collection rule\. This rule type takes a list of character attributes for all players and checks against the maximum counts\. Use `flatten` to create a list for all players in all teams\.
+ When evaluating based on latency, note the following: 
  + Latency data is provided in the matchmaking request as part of the Player object\. It is not a player attribute, so it does not need to be listed as one\.
  + The matchmaker evaluates latency by region\. Any region with a latency higher than the maximum is ignored\. To be accepted for a match, a player must have at least one region with a latency below the maximum\.
  + If a matchmaking request omits latency data one or more players, the request is rejected for all matches\.

```
{
    "name": "three_team_game",
    "ruleLanguageVersion": "1.0",
    "playerAttributes": [{
        "name": "skill",
        "type": "number",
        "default": 10
    },{
        "name": "character",
        "type": "string_list",
        "default": [ "peasant" ]
    }],
    "teams": [{
        "name": "trio",
        "minPlayers": 3,
        "maxPlayers": 5,
        "quantity": 3
    }],
    "rules": [{
        "name": "FairTeamSkill",
        "description": "The average skill of players in each team is within 10 points from the average skill of players in the match",
        "type": "distance",
        // get players for each team, and average separately to produce list of 3
        "measurements": [ "avg(teams[*].players.attributes[skill])" ],
        // get players for each team, flatten into a single list, and average to produce overall average
        "referenceValue": "avg(flatten(teams[*].players.attributes[skill]))",
        "maxDistance": 10 // minDistance would achieve the opposite result
    }, {
        "name": "CloseTeamSizes",
        "description": "Only launch a game when the team sizes are within 1 of each other.  e.g. 3 v 3 v 4 is okay, but not 3 v 5 v 5",
        "type": "distance",
        "measurements": [ "max(count(teams[*].players))"],
        "referenceValue": "min(count(teams[*].players))",
        "maxDistance": 1
    }, {
        "name": "OverallMedicLimit",
        "description": "Don't allow more than 5 medics in the game",
        "type": "collection",
        // This is similar to above, but the flatten flattens everything into a single
        // list of characters in the game.
        "measurements": [ "flatten(teams[*].players.attributes[character])"],
        "operation": "contains",
        "referenceValue": "medic",
        "maxCount": 5
    }, {
        "name": "FastConnection",
        "description": "Prefer matches with fast player connections first",
        "type": "latency",
        "maxLatency": 50
    }],
    "expansions": [{
        "target": "rules[FastConnection].maxLatency",
        "steps": [{
            "waitTimeSeconds": 10,
            "value": 100
        }, {
            "waitTimeSeconds": 20,
            "value": 150
        }]
    }]
}
```

## Example 4: Use Explicit Sorting to Find Best Matches<a name="match-examples-4"></a>

This example sets up a simple match with two teams of three players\. It illustrates how to use explicit sorting rules to help find the best possible matches as quickly as possible\. These rules presort all active matchmaking tickets to create the best matches based on certain key requirements\. This scenario is implemented with the following instructions:
+ Create two teams of players\.
+ Include exactly three players in each team\.
+ Include the following player attributes:
  + Experience level \(if not provided, default to 50\)\.
  + Preferred game modes \(can list multiple values\) \(if not provided, default to “coop” and “deathmatch”\)\.
  + Preferred game maps, including map name and preference weighting \(if not provided, default to `"defaultMap"` with a weight of 100\)\.
+ Set up presorting:
  + Sort players based on their preference for the same game map as the anchor player\. Players can have multiple favorite game maps, so this example uses a preference value\. 
  + Sort players based on how closely their experience level matches the anchor player\. With this sort, all players in all teams will have experience levels that are as close as possible\. 
+ All players across all teams must have selected at least one game mode in common\.
+ All players across all teams must have selected at least one game map in common\. 

Notes on using this rule set: 
+ The game map sort uses an absolute sort that compares the mapPreference attribute value\. Because it is first in the rule set, this sort is performed first\. 
+ The experience sort uses a distance sort to compare a prospective player's skill level with the anchor player's skill\. 
+ Sorts are performed in the order they are listed in a rule set\. In this scenario, players are sorted by game map preference, and then by experience level\. 

```
{
    "name": "multi_map_game",
    "ruleLanguageVersion": "1.0",
    "playerAttributes": [{
        "name": "experience",
        "type": "number",
        "default": 50
    }, {
        "name": "gameMode",
        "type": "string_list",
        "default": [ "deathmatch", "coop" ]
    }, {
        "name": "mapPreference",
        "type": "string_number_map",
        "default": { "defaultMap": 100 }
    }, {
        "name": "acceptableMaps",
        "type": "string_list",
        "default": [ "defaultMap" ]
    }],
    "teams": [{
        "name": "red",
        "maxPlayers": 3,
        "minPlayers": 3
    }, {
        "name": "blue",
        "maxPlayers": 3,
        "minPlayers": 3
    }],
    "rules": [{
        // We placed this rule first since we want to prioritize players preferring the same map
        "name": "MapPreference",
        "description": "Favor grouping players that have the highest map preference aligned with the anchor's favorite",
        // This rule is just for sorting potential matches.  We sort by the absolute value of a field.
        "type": "absoluteSort",
        // Highest values go first
        "sortDirection": "descending",
        // Sort is based on the mapPreference attribute.
        "sortAttribute": "mapPreference",
        // We find the key in the anchor's mapPreference attribute that has the highest value.
        // That's the key that we use for all players when sorting.
        "mapKey": "maxValue"
    }, {
        // This rule is second because any tie-breakers should be ordered by similar experience values
        "name": "ExperienceAffinity",
        "description": "Favor players with similar experience",
        // This rule is just for sorting potential matches.  We sort by the distance from the anchor.
        "type": "distanceSort",
        // Lowest distance goes first
        "sortDirection": "ascending",
        "sortAttribute": "experience"
    }, {
        "name": "SharedMode",
        "description": "The players must have at least one game mode in common",
        "type": "collection",
        "operation": "intersection",
        "measurements": [ "flatten(teams[*].players.attributes[gameMode])"],
        "minCount": 1
    }, {
        "name": "MapOverlap",
        "description": "The players must have at least one map in common",
        "type": "collection",
        "operation": "intersection",
        "measurements": [ "flatten(teams[*].players.attributes[acceptableMaps])"],
        "minCount": 1
    }]
}
```

## Example 5: Find Intersections Across Multiple Player Attributes<a name="match-examples-5"></a>

This example illustrates how to use a collection rule to find intersections in two or more player attributes\. When working with collections, you can use the `intersection` operation for a single attribute, and the `reference_intersection_count` operation for multiple attributes\. 

To illustrate this approach, this example evaluates players in a match based on their character preferences\. The example game is a "free\-for\-all" style in which all players in a match are opponents\. Each player is asked to \(1\) choose a character for themselves, and \(2\) choose characters they want to play against\. We need a rule that ensures that every player in a match is using a character that is on all other players' preferred opponents list\. 

The example rule set describes a match with the following characteristics: 
+ Team structure: One team of five players
+ Player attributes: 
  + *myCharacter*: The player's chosen character\.
  + *preferredOpponents*: List of characters that the player wants to play against\.
+ Match rules: A potential match is acceptable if each character in use is on every player's preferred opponents list\. 

To implement the match rule, this example uses a collection rule with the following property values:
+ Operation – Uses `reference_intersection_count` operation to evaluate how each string list in the measurement value intersects with the string list in the reference value\. 
+ Measurement – Uses the `flatten` property expression to create a list of string lists, with each string list containing one player's *myCharacter* attribute value\. 
+ Reference value – Uses the `set_intersection` property expression to create a string list of all *preferredOpponents* attribute values that are common to every player in the match\.
+ Restrictions – `minCount` is set to 1 to ensure that each player's chosen character \(a string list in the measurement\) matches at least one of the preferred opponents common to all players\. \(a string in the reference value\)\. 
+ Expansion – If a match is not filled within 15 seconds, relax the minimum intersection requirement\.

The process flow for this rule is as follows:

1. A player is added to the prospective match\. The reference value \(a string list\) is recalculated to include intersections with the new player's preferred opponents list\. The measurement value \(a list of string lists\) is recalculated to add the new player's chosen character as a new string list\.

1. Amazon GameLift verifies that each string list in the measurement value \(the players' chosen characters\) intersects with at least one string in the reference value \(the players' preferred opponents\)\. Since in this example each string list in the measurement contains only one value, the intersection is either 0 or 1\.

1. If any string list in the measurement does not intersect with the reference value string list, the rule fails and the new player is removed from the prospective match\.

1. If a match is not filled within 15 seconds, drop the opponent match requirement to fill the remaining player slots in the match\.

```
{
    "name": "preferred_characters",
    "ruleLanguageVersion": "1.0",

    "playerAttributes": [{
        "name": "myCharacter",
        "type": "string_list"
    }, {
        "name": "preferredOpponents",
        "type": "string_list"
    }],

    "teams": [{
        "name": "red",
        "minPlayers": 5,
        "maxPlayers": 5
    }],

    "rules": [{
        "description": "Make sure that all players in the match are using a character that is on all other players' preferred opponents list.",
        "name": "OpponentMatch",
        "type": "collection",
        "operation": "reference_intersection_count",
        "measurements": ["flatten(teams[*].players.attributes[myCharacter])"],
        "referenceValue": "set_intersection(flatten(teams[*].players.attributes[preferredOpponents]))",
        "minCount":1
    }],
    "expansions": [{
        "target": "rules[OpponentMatch].minCount",
        "steps": [{
            "waitTimeSeconds": 15,
            "value": 0
        }]
    }]
}
```

## Example 6: Compare Attributes Across All Players<a name="match-examples-6"></a>

This example illustrates how to compare player attributes across a group of players\. 

The example rule set describes a match with the following characteristics: 
+ Team structure: Two single\-player teams
+ Player attributes: 
  + *gameMode*: Type of game chosen by the player \(if not provided, default to "turn\-based"\)\.
  + *gameMap*: Game world chosen by the player \(if not provided, default to 1\)\.
  + *character*: Character chosen by the player \(no default value means that players must specify a character\)\.
+ Match rules: Matched players must meet the following requirements: 
  + Players must choose the same game mode\.
  + Players must choose the same game map\.
  + Players much choose different characters\.

Notes on using this rule set: 
+ To implement the match rule, this example uses comparison rules to check all players' attribute values\. For game mode and map, the rule verifies that the values are the same\. For character, the rule verifies that the values are different\. 
+ This example uses one player definition with a quantity property to create both player teams\. The team are assigned the following names: "player\_1" and "player\_2"\.

```
{
    "name": "",
    "ruleLanguageVersion": "1.0",

    "playerAttributes": [{
        "name": "gameMode",
        "type": "string",
        "default": "turn-based"
    }, {
        "name": "gameMap",
        "type": "number",
        "default": 1
    }, {
        "name": "character",
        "type": "number"
    }],

    "teams": [{
        "name": "player",
        "minPlayers": 1,
        "maxPlayers": 1,
        "quantity": 2
    }],

    "rules": [{
        "name": "SameGameMode",
        "description": "Only match players when they choose the same game type",
        "type": "comparison",
        "operation": "=",
        "measurements": ["flatten(teams[*].players.attributes[gameMode])"]
    }, {
        "name": "SameGameMap",
        "description": "Only match players when they're in the same map",
        "type": "comparison",
        "operation": "=",
        "measurements": ["flatten(teams[*].players.attributes[gameMap])"]
    }, {
        "name": "DifferentCharacter",
        "description": "Only match players when they're using different characters",
        "type": "comparison",
        "operation": "!=",
        "measurements": ["flatten(teams[*].players.attributes[character])"]
    }]
}
```

## Example 7: Create a Large Match<a name="match-examples-7"></a>

This example illustrates how to set up a rule set for matches that can exceed 40 players\. When a rule set describes teams with a total maxPlayer count greater than 40, it is processed as a large match\. Learn more in [ Design a FlexMatch Large\-Match Rule Set If your rule set can create matches with more than 40 players, FlexMatch processes match requests that use that rule set as large matches\. Large matches are processed using a different algorithm that significantly reduces the amount of time required to match high numbers of players\. To determine whether your rule set creates large matches, look at the *maxPlayer* setting for all teams in your rule set\. If the total of these settings exceeds 40, you've got a large match rule set\. Large match rule sets can create matches up to 200 players\.  A large match rule set uses the same components as other rule sets, with a few adjustments\. In addition, the rule set must include an algorithm component\. Review the schema for a large match rule set in [Rule Set Schema for Large Matches](match-ruleset-schema.md#match-ruleset-schema-large)\.  Define Large Match Algorithm Add an algorithm component to the rule set\. This component configures the large\-match algorithm for your preferences\.   *batchingPreference* \(required\) – This property indicates the way that players are sorted during match creation\. The value must be equal to “balanced”\.  *balancedAttribute *\(required\) – Identify a single player attribute to use when choosing players for a match\. Before evaluating individual players, FlexMatch sorts the available player pool according to this attribute\. It starts by evaluating players that have similar attribute values, and gradually moves on to players that are less similar until the match is filled\. This attribute is used to achieve a player balance in the match, grouping players that tend to have similar values for this attribute\. For example, if you choose a skill attribute, players with similar skill levels are grouped together in a match\. This mechanism is most effective for large matches when you have large pools of available players\. Be sure to declare the balancing attribute in the rule set's player attributes\. Only attributes with data type "number" can be used as a balancing attribute\.   *strategy *\(required\) – Choose a matching strategy to use during match creation\. Options are "largestPopulation" \(the default\) and "fastestRegion"\.   Largest population With this strategy, FlexMatch maintains the largest possible player pool by including all players who have acceptable latency values in at least one region\. With a large player pool, matches tend to fill more quickly, and matched players are more similar with regard to the balancing attribute\. Players may be placed in games where their latency is less than ideal, although still within acceptable limits\.   Fastest region This strategy places a priority on getting players into matches that deliver the best possible latency for them\. FlexMatch groups available players based on the regions where they report lowest latency values, and then tries to fill matches from these groups\. FlexMatch favors placing players in the fastest possible regions for them\. However, it may group players based on their second\- or third\-fastest regions \(or slower\) in order to create groups large enough to fill a match\. As a result, matches may take longer to fill\. In addition, players in a match that is created with this strategy may vary more widely with regard to the balancing attribute\.    Here's an example: 

```
"algorithm": {
    "balancedAttribute": "player_skill",
    "strategy": "balanced",
    "batchingPreference": "largestPopulation"
},
```   Declare Player Attributes At a minimum, you must declare the player attribute that is used as a balancing attribute in the rule set algorithm\. Only attributes with data type "number" can be used as the balancing attribute\. In addition, you may want to pass certain player attributes to the game server to use when setting up the game session\. For example, you could pass a player's character choice, map preference, etc\. To pass on player attributes, declare them in the rule set and then include attribute values for each player in your matchmaking requests\. When GameLift passes the game session request to the game server, it includes the matchmaker data, which contains attribute values for all matched players\.   Define Teams The process of defining team size and structure is the same as with small matches, but the way FlexMatch fills the teams is different\. This affects how what matches are likely to look like when only partially filled\. You may want to alter your minimum team sizes in response\. FlexMatch uses the following rules when assigning a player to a team\. First: look for teams that haven't yet reached their minimum player requirement\. Second: of those teams, find the one with the most open slots\.  For matches that define multiple equally sized teams, players are added sequentially to each team until full\. As a result, matches have similar numbers of players assigned to each team, even if the match is not full\. There is currently no way to force equally sized teams in large matches\. For matches with asymmetrically sized teams, the process is a bit more complex\. In this scenario, players initially get assigned to the largest teams with the most open slots\. Then, as the number of open slots become more evenly distributed across all teams, players start getting added to the smaller teams\. Lets walk through an example\. Say you have a rule set with three teams\. The Red and Blue teams are both set to maxPlayers=10, minPlayers=5\. The Green team is set to maxPlayers=3, minPlayers=2\. Here's the sequence for filling this match:   No team has reached minPlayers\. Red and Blue teams have 10 open slots, while Green has 3\. The first 10 players are assigned \(5 each\) to the Red and Blue teams\. Both teams have now reached minPlayers\. Green team has not yet reached minPlayers\. The next 2 players are assigned to the Green team\. All teams have now reached minPlayers\. Red and Blue teams have the most open slots, so the next 8 players are assigned \(4 each\) to the Red and Blue teams\. Once all three teams have 1 open slot available, the remaining 3 player slots are assigned in no particular order\.    Set Latency Rule for Large Matches Since most of the work of creating large matches is done using the balancing player attribute and prioritization strategy\. Most custom rules are not available\. However, you can create a rule that sets a hard limit on player latency\.  To create this rule, use the `latency` rule type with the property *maxLatency*\. Here's an example that sets maximum player latency to 200 milliseconds: 

```
"rules": [{
        "name": "player-latency",
        "type": "latency",
        "maxLatency": 200
    }],
```   Relax Large Match Requirements As with small matches, you can use expansions to relax match requirements over time when no valid matches are possible\. With large matches, you have the option to relax either latency rules or team player counts\.  If you're using automatic match backfill for large matches, avoid relaxing your team player counts too quickly\. FlexMatch starts generating backfill requests only after a game session starts, which may not happen for several seconds after a match is created\. During that time, FlexMatch can create multiple partially filled new game sessions, especially when the player count rules are lowered\. As a result, you end up with more game sessions than you need and players spread too thinly across them\. Best practice is to give the first step in your player count expansion a longer wait time, long enough for your game session to start\. Since backfill requests are given higher priority with large matches, incoming players will be slotted into existing games before new game are started\. You may need to experiment to find the ideal wait time for your game\. Here's an example that gradually lowers the Yellow team's player count, with a longer initial wait time\. Keep in mind that wait times in rule set expansions are absolute, not compounded\. So the first expansion occurs at five seconds, and the second expansion occurs five seconds later, at ten seconds\. 

```
"expansions": [{
        "target": "teams[Yellow].minPlayers",
        "steps": [{
            "waitTimeSeconds": 5,
            "value": 8
        }, {
            "waitTimeSeconds": 10,
            "value": 5
        }]
    }]
```  ](match-design-ruleset.md#match-design-rulesets-large)\. 

The example rule set creates a match using the following instructions: 
+ Create one team with up to 200 players, with a minimum requirement of 175 players\. 
+ Balancing criteria: Select players based on similar skill level\. All players must report their skill level to be matched\.
+ Batching preference: Group players by similar balancing criteria when creating matches\. 
+ Latency rules: Set the maximum acceptable player latency of 150 milliseconds\.
+ If the match is not filled quickly, relax the requirements to complete a match in reasonable time\. 
  + After 10 seconds, accept a team with 150 players\. 
  + After 12 seconds, raise the maximum acceptable latency to 200 milliseconds\. 
  + After 15 seconds, accept a team with 100 players\.

Notes on using this rule set: 
+ Because the algorithm uses the "largest population" batching preference, players are first sorted based on the balancing criteria\. As a result, matches tend to be fuller and contain players that are more similar in skill\. All players meet acceptable latency requirements, but they may not get the best possible latency for their location\.
+ The algorithm strategy used in this rule set, "largest population", is the default setting\. To use the default, you can opt to omit the setting\.
+ If you've enabled match backfill, do not relax player count requirements too quickly, or you may end up with too many partially filled game sessions\. Learn more in [Relax Large Match Requirements](match-design-ruleset.md#match-design-rulesets-large-relax)\.

```
{
    "name": "free-for-all",
    "ruleLanguageVersion": "1.0",
    "playerAttributes": [{
        "name": "skill",
        "type": "number"
    }],
    "algorithm": {
        "balancedAttribute": "skill",
        "strategy": "balanced",
        "batchingPreference": "largestPopulation"
    },
    "teams": [{
        "name": "Marauders",
        "maxPlayers": 200,
        "minPlayers": 175
    }],
    "rules": [{
        "name": "low-latency",
        "description": "Sets maximum acceptable latency",
        "type": "latency",
        "maxLatency": 150
    }],
    "expansions": [{
        "target": "rules[low-latency].maxLatency",
        "steps": [{
            "waitTimeSeconds": 12,
            "value": 200
        }],
        "target": "teams[Marauders].minPlayers",
        "steps": [{
            "waitTimeSeconds": 10,
            "value": 150
        }, {
            "waitTimeSeconds": 15,
            "value": 100
        }]
    }]
}
```

## Example 8: Create a Multi\-team Large Match<a name="match-examples-8"></a>

This example illustrates how to set up a rule set for matches with multiple teams that can exceed 40 players\. This example illustrates how to create multiple identical teams with one definition and how asymmetrically sized teams are filled during match creation\.

The example rule set creates a match using the following instructions: 
+ Create ten identical "hunter" teams with up to 15 players, and one "monster" team with exactly 5 players\. 
+ Balancing criteria: Select players based on number of monster kills\. If players don't report their kill count, use a default value of 5\.
+ Batching preference: Group players based on the regions where they report the fastest player latency\. 
+ Latency rule: Sets a maximum acceptable player latency of 200 milliseconds\. 
+ If the match is not filled quickly, relax the requirements to complete a match in reasonable time\. 
  + After 15 seconds, accept teams with 10 players\. 
  + After 20 seconds, accept teams with 8 players\. 

Notes on using this rule set: 
+ This rule set defines teams that can potentially hold up to 155 players, which makes it a large match\. \(10 x 15 hunters \+ 5 monsters = 155\)
+ Because the algorithm uses the "fastest region" batching preference, players tend to be placed in regions where they report faster latency and not in regions where they report high \(but acceptable\) latency\. At the same time, matches are likely to have fewer players, and the balancing criteria \(number of monster skills\) may vary more widely\.
+ When an expansion is defined for a multi\-team definition \(quantity > 1\), the expansion applies to all teams created with that definition\. So by relaxing the hunter team minimum players setting, all ten hunter teams are affected equally\.
+ Since this rule set is optimized to minimize player latency, the latency rule acts as a catch\-all to exclude players who have no acceptable connection options\. We don't need to relax this requirement\.
+ Here's how FlexMatch fills matches for this rule set before any expansions take effect:
  + No teams have reached minPlayers count yet\. Hunter teams have 15 open slots, while Monster team has 5 open slots\. 
    + The first 100 players are assigned \(10 each\) to the ten hunter teams\.
    + The next 22 players are assigned sequentially \(2 each\) to hunter teams and monster team\.
  + Hunter teams have reached minPlayers count of 12 players each\. Monster team has 2 players and has not reached minPlayers count\.
    + The next three players are assigned to the monster team\.
  + All teams have reached minPlayers count\. Hunter teams each have three open slots\. Monster team is full\.
    + The final 30 players are assigned sequentially to the hunter teams, ensuring that all hunter teams have nearly the same size \(plus or minus one player\)\.
+ If you've enabled backfill for matches created with this rule set, do not relax player count requirements too quickly, or you may end up with too many partially filled game sessions\. Learn more in [Relax Large Match Requirements](match-design-ruleset.md#match-design-rulesets-large-relax)\.

```
{
    "name": "monster-hunters",
    "ruleLanguageVersion": "1.0",
    "playerAttributes": [{
        "name": "monster-kills",
        "type": "number",
        "default": 5
    }],
    "algorithm": {
        "balancedAttribute": "monster-kills",
        "strategy": "balanced",
        "batchingPreference": "fastestRegion"
    },
    "teams": [{
        "name": "Monsters",
        "maxPlayers": 5,
        "minPlayers": 5
    }, {
        "name": "Hunters",
        "maxPlayers": 15,
        "minPlayers": 12,
        "quantity": 10
    }],
    "rules": [{
        "name": "latency-catchall",
        "description": "Sets maximum acceptable latency",
        "type": "latency",
        "maxLatency": 150
    }],
    "expansions": [{
        "target": "teams[Hunters].minPlayers",
        "steps": [{
            "waitTimeSeconds": 15,
            "value": 10
        }, {
            "waitTimeSeconds": 20,
            "value": 8
        }]
    }]
}
```