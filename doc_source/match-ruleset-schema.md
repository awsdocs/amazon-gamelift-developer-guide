# FlexMatch Rule Set Schema<a name="match-ruleset-schema"></a>

This topic documents the standard schema for a small\-match rule set and a large\-match rule set\. Use the rules language, detailed in [ FlexMatch Rules LanguageRules Language  Look up property expression syntax when writing rules in Amazon GameLift FlexMatch rule sets\.   Use the following property expression syntax when writing rules for FlexMatch rule sets\. Learn more about creating FlexMatch rules:   [Build a FlexMatch Rule Set](match-rulesets.md)   [Create Matchmaking Rule Sets](match-create-ruleset.md)   [FlexMatch Rule Set Examples](match-examples.md)    Rule Types FlexMatch supports the following rule types\. Each rule time has a set of properties, which are described here\.  

**Distance rule \(`distance`\)**  
Distance rules measure the difference between two number values, such as the distance between skill levels\. For example, a distance rule might require that all players be within two levels of each other\.  
**Distance rule properties**  
+ **`measurements`** – Player attribute value to measure distance for; must be a number value\. 
+ **`referenceValue`** – Number value to measure distance against for a prospective match\.
+ **`minDistance`/`maxDistance`** – Maximum or minimum distance value allowed for a successful match\.
+ **`partyAggregation`** – How to handle multiple\-player \(party\) requests\. Valid options are to use the minimum \(`min`\), maximum \(`max`\), or average \(`avg`\) values for a request's players\. Default is `avg`\.  

**Comparison rule \(`comparison`\)**  
Comparison rules compare a player attribute value against another value\. There are two types of comparison rules\. The first type compares an attribute value to a reference value\. Specify the reference value and any valid comparison operation\. For example, the rule might require that matched players have skill level 24 or greater\. The second type compares an attribute value across all players in a team or match\. This type omits the reference value and specifies either equals or not\-equals \(all or none of the players have the same attribute value\)\. For example, the rule might require that all players choose the same game map\.  
**Comparison rule properties**  
+ **`measurements`** – Player attribute value to compare\.
+ **`referenceValue`** – Value to evaluate the measurement against for a prospective match\. 
+ **`operation`** – How to evaluate the measurement\. Valid operations include: `<, <=, =, !=, >, >=`\.
+ **`partyAggregation`** – How to sort multiple\-player \(party\) requests\. Valid options are to use the minimum \(`min`\), maximum \(`max`\), or average \(`avg`\) values for a request's players\. Default is `avg`\.  

**Collection rule \(`collection`\)**  
Collection rules evaluate collections of player attribute values\. A collection might contain attribute values for multiple players, a player attribute that is a collection \(a string list\), or both\. For example, a collection rule might look at the collection of characters chosen by a team's players and require that it contain at least one of a certain character\.  
**Collection rule properties**  
+ **`measurements`** – Collection of player attribute values to evaluate\. Attribute values must be string lists\. 
+ **`referenceValue`** – Value or collection of values to evaluate the measurements against for a prospective match\. 
+ **`operation`** – How to evaluate a measurement collection\. Valid operations include:
  + `intersection` measures the number of values that are common in all players' collections\. See the MapOverlap rule in [Example 4: Use Explicit Sorting to Find Best Matches](match-examples.md#match-examples-4)\.
  + `contains` measures the number of player attribute collections that contain a certain reference value\. See the OverallMedicLimit rule in [Example 3: Set Team\-Level Requirements and Latency Limits](match-examples.md#match-examples-3)\.
  + `reference_intersection_count` measures the intersection between a player attribute collection and a reference value collection\. Each player attribute collection \(string list\) in the measurements is evaluated against the reference collection separately\. This operation can be used to evaluate across different player attributes\. See the OpponentMatch rule in [Example 5: Find Intersections Across Multiple Player Attributes](match-examples.md#match-examples-5)\.
+ **`minCount`/`maxCount`** – Maximum or minimum count value allowed for a successful match\. 
+ **`partyAggregation`** – How to sort multiple\-player \(party\) requests\. Valid options are to use the `union` or `intersection` of values for a request's players\. Default is `union`\. 

**Latency rule \(`latency`\)**  
Latency rules evaluate player latency settings for acceptable matches\. For example, a rule might require all matched players must have a regional latency under a maximum limit\. Currently, this is the only rule type you can use in a rule set for large matches, and setting `maxLatency` is the only available option\.  
**Latency rule properties**  
+ **`maxLatency`** – Highest acceptable latency value for a region\. For each player, ignore all regions that exceed this latency\. 
+ **`maxDistance`** – Maximum difference between the latency of each player and the distance reference value\.
+ **`distanceReference`** – Use with `maxDistance`\. Number value to measure distance against for a successful match\. For latency, this value is an aggregate of latency values for multiple players\. Valid options are minimum \(`min`\) or average \(`avg`\) player latency value\. \(See the Property Expressions section\.\)
+ **`partyAggregation`** – How to sort multiple\-player \(party\) match requests\. Valid options are to use the minimum \(`min`\), maximum \(`max`\), or average \(`avg`\) values for a request's players\. Default is `avg`\.  

**Distance sort rule \(`distanceSort`\)**  
Distance sort is an explicit sort option that directs the matchmaker to pre\-sort matchmaking requests based on a player attribute\. A distance sort rule evaluates matchmaking requests based on the distance from the anchor request\.  
**Distance sort rule properties**  
+ **`sortDirection`** – Direction to sort matchmaking requests\. Valid options are `ascending` or `descending`\.
+ **`sortAttribute`** – Player attribute to sort players by\. 
+ **`mapKey`** – How to evaluate the player attribute if it's a map\. Valid options include: 
  + `minValue`: For the anchor player, find the key with the lowest value\.
  + `maxValue`: For the anchor player, find the key with the highest value\.
+ **`partyAggregation`** – How to sort multiple\-player \(party\) requests\. Valid options are to use the minimum \(`min`\), maximum \(`max`\), or average \(`avg`\) values for a request's players\. Default is `avg`\.  

**Absolute sort rule \(`absoluteSort`\)**  
Absolute sort is an explicit sort option that directs the matchmaker to pre\-sort matchmaking requests based on a player attribute\. An absolute sort evaluates matchmaking requests based on whether its player attribute matches that of the anchor request\.   
**Absolute sort rule properties**  
+ **`sortDirection`** – Direction to sort matchmaking requests\. Valid options are `ascending` or `descending`\.
+ **`sortAttribute`** – Player attribute to sort players by\. 
+ **`mapKey`** – How to evaluate the player attribute if it's a map\. Valid options include: 
  + `minValue`: For the anchor player, find the key with the lowest value\.
  + `maxValue`: For the anchor player, find the key with the highest value\.
+ **`partyAggregation`** – How to sort multiple\-player \(party\) requests\. Valid options are to use the minimum \(`min`\), maximum \(`max`\), or average \(`avg`\) values for a request's players\. Default is `avg`\.     Property Expressions Property expressions are used in a rule set to refer to certain matchmaking\-related properties\. Properties can include the player attribute values from matchmaking requests\. For example, a rule might use a property expression to identify which player attribute to evaluate\. They generally take one of two forms:   Individual player data  Calculated team data, which takes the form of collections of individual player data\.  A valid property expression identifies a specific value for a single player, team, or match\. The following partial expressions illustrate how to identify teams and players:  


|  |  |  |  | 
| --- |--- |--- |--- |
| To identify a specific team in a match: | teams\[red\] | The Red team | Team  | 
| To identify all teams in a match: | teams\[\*\] | All teams | List<Team> | 
| To identify players in a specific team: | team\[red\]\.players | Players in the Red team  | List<Player> | 
| To identify players in a match: | team\[\*\]\.players | Players in the match, grouped by team | List<List<Player>> |  The following table illustrates some valid property expressions that build on the previous examples: 


****  

| Expression | Meaning | Resulting Type | 
| --- | --- | --- | 
|  teams\[red\]\.players\[playerid\]  | The player IDs of all players on the red team | List<string> | 
| teams\[red\]\.players\.attributes\[skill\] | The "skill" attributes of all players on the red team | List<number> | 
| teams\[\*\]\.players\.attributes\[skill\] | The "skill" attributes of all players in the match, grouped by team | List<List<number>> |  Property expressions can be used to aggregate team data by using the following functions or combinations of functions: 


****  

| Aggregation | Input | Meaning | Output | 
| --- | --- | --- | --- | 
| min | List<number> | Get the minimum of all numbers in the list\. | number | 
| max | List<number> | Get the maximum of all numbers in the list\. | number | 
| avg | List<number> | Get the average of all numbers in the list\. | number | 
| median | List<number> | Get the median of all numbers in the list\. | number | 
| sum | List<number> | Get the sum of all numbers in the list\. | number | 
| count | List<?> | Get the number of elements in the list\. | number | 
| stddev | List<number> | Get the standard deviation of all numbers in the list\. | number | 
| flatten | List<List<?>> | Turn a collection of nested lists into a single list containing all elements\. | List<?> | 
| set\_intersection | List<List<string>> | Get a list of strings that are found in all string lists in a collection\. | List<string> | 
| All above | List<List<?>> | All operations on a nested list operate on each sublist individually to produce a list of results\. | List<?> |  The following table illustrates some valid property expressions that use aggregation functions:  


****  

| Expression | Meaning | Resulting Type | 
| --- | --- | --- | 
| flatten\(teams\[\*\]\.players\.attributes\[skill\]\) | The "skill" attributes of all players in the match \(not grouped\) | List<number> | 
| avg\(teams\[red\]\.players\.attributes\[skill\]\) | The average skill of the red team players | number | 
| avg\(teams\[\*\]\.players\.attributes\[skill\] | The average skill of each team in the match | List<number> | 
| avg\(flatten\(teams\[\*\]\.players\.attributes\[skill\]\)\) | The average skill level of all players in the match\. This expression gets a flattened list of player skills and then averages them\. | number | 
| count\(teams\[red\]\.players\) | The number of players on the red team | number | 
| count \(teams\[\*\]\.players\) | The number of players on each team in the match | List<number> | 
| max\(avg\(teams\[\*\]\.players\.attributes\[skill\]\)\) | The highest team skill level in the match  | number |   ](match-rules-reference.md) to develop your custom values\.

## Rule Set Schema for Small Matches<a name="match-ruleset-schema-small"></a>

Use this schema when creating a rule set to build matches of 40 players maximum\.

```
{
    "name": <descriptive label, string>,
    "ruleLanguageVersion": <must be "1.0">,
    "playerAttributes":[{ 
         "name": <unique name for player attribute to be used by matchmaker, string>,
         "type": <attribute data type, allowed values are "string", "number", "string_list", "string_number_map">,
         "default": <value to use when no player-specific value is provided>
    }],
    "teams": [{
        "name": <unique label, string>,
        "maxPlayers": <max players allowed in team>,
        "minPlayers": <min players required in team>,
        "quantity": <number of teams to create with this definition>
    }],
    "rules": [{
        "name": <unique label, string>,
        "description": <descriptive label, string>,
        "type": <rule type, string>,
        "<type-specific property>": <property expression>
    }],
    "expansions": [{
        "target": <rule/team and property to adjust value for, example: "rules[<minSkill>].referenceValue">,
        "steps": [{
            "waitTimeSeconds": <length of 1st wait period before relaxing rule>,
            "value": <new value>
        }, {
            "waitTimeSeconds": <length of 2nd wait period before further relaxing rule>,
            "value": <new value>
        }]
    }]
}
```

## Rule Set Schema for Large Matches<a name="match-ruleset-schema-large"></a>

Use this schema when creating a rule set to build matches of greater than 40 players\. If the `maxPlayers` values for all teams defined in the rule set exceeds 40, then GameLift processes all requests that use this rule sets under large\-match guidelines\. 

```
{
    "name": <descriptive label, string>,
    "ruleLanguageVersion": <must be "1.0">,
    "playerAttributes":[{ 
         "name": <unique name for player attribute to be used by matchmaker, string>,
         "type": <attribute data type, allowed values are "string", "number", "string_list", "string_number_map">,
         "default": <value to use when no player-specific value is provided>
    }],
    "teams": [{ 
        "name": <unique label, string>,
        "maxPlayers": <max players allowed in team>,
        "minPlayers": <min players required in team>,
        "quantity": <number of teams to create with this team definition>
    }],
    "algorithm": {
        "balancedAttribute": <name of player attribute, data type "number", to use when grouping players >,
        "strategy": <must be "balanced">,
        "batchingPreference": <choose between "largestPopulation" (default) or "fastestRegion">
    },
    "rules": [{
        "name": <unique label, string>,
        "description": <descriptive label, string>,
        "type": <rule type, must be "latency">,
        "<type-specific property>": <property expression, must set value for "maxLatency">
    }],
    "expansions": [{
        "target": <rule/team and property to adjust value for, example: "rules[<rule name>].maxLatency">,
        "steps": [{
            "waitTimeSeconds": <length of 1st wait period before relaxing rule>,
            "value": <new value>
        }, {
            "waitTimeSeconds": <length of 2nd wait period before further relaxing rule>,
            "value": <new value>
        }]
    }]
}
```