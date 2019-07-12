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

## Example 3: Set Team Requirements and Latency Limits<a name="match-examples-3"></a>

This example illustrates how to set up three equally matched teams of players and apply a set of rules to a team instead of each individual player\. It also establishes a maximum latency for all players\. Latency maximums can be relaxed over time to complete the match\. This scenario sets out the following instructions:
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
+ Only match players with 50ms latency at most\.
+ If a match is not filled quickly, relax the player latency requirement as follows: 
  + After 10 seconds, allow player latency values up to 100 ms\.
  + After 20 seconds, allow player latency values up to 150 ms\. 

Notes on using this rule set: 
+ The rule set ensures that teams are evenly matched based on player skill\. To evaluate the `FairTeamSkill` rule, FlexMatch tentatively adds the prospective player to a team, calculates the average skill of players in the team, and compares it against the average skill of players in both teams\. If rule fails, the prospective player is not added to the match\.
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
        "name": "red",
        "minPlayers": 3,
        "maxPlayers": 5
    }, {
        "name": "blue",
        "minPlayers": 3,
        "maxPlayers": 5
    }, {
        "name": "green",
        "minPlayers": 3,
        "maxPlayers": 5
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
+ The game map sort uses an absolute sort that compares the mapPreference attribute value\. It is placed first in the rule set to prioritize it\. 
+ The experience sort uses a distance sort that compares the distance of a prospective player from the anchor player\. 
+ The order of the sorting rules determines the order that each sort is performed\. In this scenario, players are sorted by game map preference, and then by experience level\. 

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

## Example 5: Find intersections across multiple player attributes<a name="match-examples-5"></a>

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
+ Team structure: Two teams of one player each
+ Player attributes: 
  + *gameMode*: Type of game chosen by the player \(if not provided, default to "turn\-based"\)\.
  + *gameMap*: Game world chosen by the player \(if not provided, default to 1\)\.
  + *character*: Character chosen by the player \(no default value means players must specify a character\)\.
+ Match rules: Matched players must meet the following requirements: 
  + Players must choose the same game mode\.
  + Players must choose the same game map\.
  + Players much choose different characters\.

To implement the match rule, this example uses comparison rules to check all players' attribute values\. For game mode and map, the rule verifies that the values are the same\. For character, the rule verifies that the values are different\. 

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
        "name": "red",
        "minPlayers": 1,
        "maxPlayers": 1
    }, {
        "name": "blue",
        "minPlayers": 1,
        "maxPlayers": 1
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