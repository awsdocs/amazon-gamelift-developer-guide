# Design a FlexMatch Rule Set<a name="match-design-ruleset"></a>

At its most basic, a matchmaking rule set does two things: it lays out a match's team structure and size, and it tells the matchmaker how to evaluate players to find the best possible match\. But it can do quite a bit more than that\. For example, rule set is also where you address matchmaking issues such as these:
+ Trigger special match processing for large matches \(>40 players\)\.
+ Enforce minimum player latency requirements to protect players' gameplay\.
+ Relax team requirements or match rules gradually if no match can be made\.
+ Define special handling for match requests that contain multiple players \(party aggregation\)\.

This topic covers the basic structure of a rule set and how to use them for matches with up to 40 players\. Matches for greater than 40 players require a different rule set structure, because FlexMatch uses a streamlined algorithm to quickly match large groups of players\. Learn more about building large matches in [Design a FlexMatch Large\-Match Rule Set](#match-design-rulesets-large)\.

**Topics**
+ [Create Matchmaking Rule Sets](match-create-ruleset.md)
+ [FlexMatch Rule Set Examples](match-examples.md)
+ [FlexMatch Rules Language](match-rules-reference.md)

## Define Rule Set Components<a name="match-rulesets-components"></a>

All rule sets specify some or all of the following components\. At a minimum, a rule set must specify the rule language version, define a team, and set one rule\. In most cases, you also want to declare a player attribute and use it in a custom rule\.

Review the general rule set schema in [Rule Set Schema for Large Matches](match-ruleset-schema.md#match-ruleset-schema-large)\.

There are additional requirements for a rule set that creates large matches \(greater than 40 players\)\. Learn more about large matches in [Design a FlexMatch Large\-Match Rule Set](#match-design-rulesets-large)\.

### Describe Rule Set<a name="match-rulesets-components-set"></a>

Provide details for the rule set\.
+ *name* \(optional\) – This is a descriptive label within the rule set syntax and is not used by Amazon GameLift in any meaningful way\. Do not confuse this value with the rule set name, which is set, along with the rule set syntax, when you create a rule set\. 
+ *ruleLanguageVersion* \(required\) – This is the version of the property expression language used to create FlexMatch rules\. The value must be equal to “1\.0”\.

### Declare Player Attributes<a name="match-rulesets-components-attributes"></a>

Rules may choose players for matches based on individual player characteristics\. If you create rules that rely on player attributes, they must be declared in this section\. Values for declared player attributes should be included in every matchmaking request that is sent a matchmaker using this rule set\.

You may also want to pass certain player attributes to the game session even if the rule set doesn't use them during player evaluation\. For example, you might pass a player's character choice\. To do this, declare your player attributes here, and include the attribute values for each player in your matchmaking requests\.

When declaring a player attribute, include the following information: 
+ *name *\(required\) – This value must be unique to the rule set\.
+ *type *\(required\) – This is the data type of the attribute value\. Valid data types are number, string, or string map\.
+ *default *\(optional\) – Enter a default value to use when no value is provided for a player\. If no default is declared and a player does not provide a value, the player cannot be matched\.

### Define Teams<a name="match-rulesets-components-teams"></a>

Describe the structure and size of the teams for a match\. Each match must have at least one team, and you can define as many teams as you want\. Your teams can have the same number of players or be asymmetric\. For example, you might define a single\-player monster team and a hunters team with 10 players\.

FlexMatch processes match requests as either small match or large match, based on how the rule set defines team sizes\. Potential matches of up to 40 players are small matches, while matches with more than 40 players are large matches\. To determine a rule set's potential match size, add up the *maxPlayer* settings for all teams defined in the rule set\. 
+ *name * \(required\) – Assign each team a unique name\. This name is used in rules and expansions, and it is referenced in the matchmaking data that is used in the game session\.
+ *maxPlayers* \(required\) – Specify the maximum number of players that can be assigned to the team\.
+ *minPlayers* \(required\) – Specify the minimum number of players that must be assigned to the team before the match can succeed\. 
+ *quantity* \(optional\) – If you want FlexMatch to create more than one team based on this definition, specify how many\. When FlexMatch creates a match, these teams are given the designated name with an appended number\. For example "Red\-Team\_1", "Red\-Team\_2", "Red\-Team\_3", etc\. 

FlexMatch always tries to fill teams to the maximum player size but does create teams with fewer players when the minimum player size allows it\. If you want all teams in the match to be equally sized, you can create a rule for that\. See the [FlexMatch Rule Set Examples](match-examples.md) topic for an example of an "EqualTeamSizes" rule\.

### Set Rules for Player Matching<a name="match-rulesets-components-rules"></a>

Create a set of rule statements that define how to evaluate players for acceptance in to a match\. Rules might set requirements that apply to individual players, teams, or an entire match\. When GameLift processes a match request, it starts with the oldest player in the pool of available players and builds a match around that player\.
+ *name* \(required\) – This is a meaningful name that uniquely identifies the rule within a rule set\. Rule names are also referenced in event logs and metrics that track activity related to this rule\. 
+ *description* \(optional\) – Use this element to attach a free\-form text description\. This information is not used by the matchmaker\.
+ *type* \(required\) – The type element identifies the operation to use when processing the rule\. Each rule type requires a set of additional properties\. For example, several rule types need a reference value to measure a player's attributes against\. See a list of valid rule types and properties in [FlexMatch Rules Language](match-rules-reference.md)\. 
+ Rule type property \(may be required\) – Depending on the type of rule being defined, you may need to set certain rule properties\. For example, with distance and comparison rules, you must specify which player attribute to measure\. Learn more about properties and how to use the FlexMatch property expression language in [FlexMatch Rules Language](match-rules-reference.md)\.

### Relax Match Requirements Over Time<a name="match-rulesets-components-expansion"></a>

Expansions allow you to relax match criteria over time when no valid matches are possible\. This feature ensures that a "best available" match can be made when a perfect match is not possible\. You might use an expansion to relax a player skill requirement, increase acceptable player latency levels, or decrease the minimum number of players required\. By relaxing your rules with an expansion, you are gradually expanding the pool of players that can be matched\. 
+ *target *\(required\) – Identify the rule set element to be relaxed\. You can relax a rule or a team property\.
+ *steps *\(required\) – You can relax a rule in multiple stages\. For each step, specify the wait time and the new value to apply\. For rule set expansions, wait times are absolute, representing a number of seconds after match creations starts\. For expansions with multiple steps, each step's wait time must be larger than the previous step\. The last step's wait time cannot be longer than the time allowed for a match request, which is set in the matchmaking configuration\.
  + *waitTimeSeconds*
  + *value*

If this rule set is used by a matchmaker that has automatic backfill enabled, don't relax your player count requirements too quickly\. It takes a few seconds for the new game session to start up and begin automatic backfill\. If you have very short wait times in your expansion steps, FlexMatch is likely to create a lot of partially\-filled matches before new games start backfilling\. A better approach is to set your expansion wait times only after automatic backfill tends to kicks in for your games\. This helps FlexMatch get players into games \(new or existing\) faster and more efficiently\. At the same time, you want FlexMatch to backfill your existing games \(using this same rule set\) as quickly as possible\. Expansion timing varies depending on your team composition\. Expect to do some testing to find the best expansion strategy for your game\.

## Design a FlexMatch Large\-Match Rule Set<a name="match-design-rulesets-large"></a>

If your rule set can create matches with more than 40 players, FlexMatch processes match requests that use that rule set as large matches\. Large matches are processed using a different algorithm that significantly reduces the amount of time required to match high numbers of players\.

To determine whether your rule set creates large matches, look at the *maxPlayer* setting for all teams in your rule set\. If the total of these settings exceeds 40, you've got a large match rule set\. Large match rule sets can create matches up to 200 players\. 

A large match rule set uses the same components as other rule sets, with a few adjustments\. In addition, the rule set must include an algorithm component\. Review the schema for a large match rule set in [Rule Set Schema for Large Matches](match-ruleset-schema.md#match-ruleset-schema-large)\.

### Define Large Match Algorithm<a name="match-design-rulesets-large-algorithm"></a>

Add an algorithm component to the rule set\. This component configures the large\-match algorithm for your preferences\.
+ *batchingPreference* \(required\) – This property indicates the way that players are sorted during match creation\. The value must be equal to “balanced”\. 

  *balancedAttribute *\(required\) – Identify a single player attribute to use when choosing players for a match\. Before evaluating individual players, FlexMatch sorts the available player pool according to this attribute\. It starts by evaluating players that have similar attribute values, and gradually moves on to players that are less similar until the match is filled\. This attribute is used to achieve a player balance in the match, grouping players that tend to have similar values for this attribute\. For example, if you choose a skill attribute, players with similar skill levels are grouped together in a match\. This mechanism is most effective for large matches when you have large pools of available players\.

  Be sure to declare the balancing attribute in the rule set's player attributes\. Only attributes with data type "number" can be used as a balancing attribute\.
+ *strategy *\(required\) – Choose a matching strategy to use during match creation\. Options are "largestPopulation" \(the default\) and "fastestRegion"\. 

**Largest population**  
With this strategy, FlexMatch maintains the largest possible player pool by including all players who have acceptable latency values in at least one region\. With a large player pool, matches tend to fill more quickly, and matched players are more similar with regard to the balancing attribute\. Players may be placed in games where their latency is less than ideal, although still within acceptable limits\.

**Fastest region**  
This strategy places a priority on getting players into matches that deliver the best possible latency for them\. FlexMatch groups available players based on the regions where they report lowest latency values, and then tries to fill matches from these groups\. FlexMatch favors placing players in the fastest possible regions for them\. However, it may group players based on their second\- or third\-fastest regions \(or slower\) in order to create groups large enough to fill a match\. As a result, matches may take longer to fill\. In addition, players in a match that is created with this strategy may vary more widely with regard to the balancing attribute\.

Here's an example:

```
"algorithm": {
    "balancedAttribute": "player_skill",
    "strategy": "balanced",
    "batchingPreference": "largestPopulation"
},
```

### Declare Player Attributes<a name="match-design-rulesets-large-attributes"></a>

At a minimum, you must declare the player attribute that is used as a balancing attribute in the rule set algorithm\. Only attributes with data type "number" can be used as the balancing attribute\.

In addition, you may want to pass certain player attributes to the game server to use when setting up the game session\. For example, you could pass a player's character choice, map preference, etc\. To pass on player attributes, declare them in the rule set and then include attribute values for each player in your matchmaking requests\. When GameLift passes the game session request to the game server, it includes the matchmaker data, which contains attribute values for all matched players\.

### Define Teams<a name="match-design-rulesets-large-teams"></a>

The process of defining team size and structure is the same as with small matches, but the way FlexMatch fills the teams is different\. This affects how what matches are likely to look like when only partially filled\. You may want to alter your minimum team sizes in response\.

FlexMatch uses the following rules when assigning a player to a team\. First: look for teams that haven't yet reached their minimum player requirement\. Second: of those teams, find the one with the most open slots\. 

For matches that define multiple equally sized teams, players are added sequentially to each team until full\. As a result, matches have similar numbers of players assigned to each team, even if the match is not full\. There is currently no way to force equally sized teams in large matches\. For matches with asymmetrically sized teams, the process is a bit more complex\. In this scenario, players initially get assigned to the largest teams with the most open slots\. Then, as the number of open slots become more evenly distributed across all teams, players start getting added to the smaller teams\.

Lets walk through an example\. Say you have a rule set with three teams\. The Red and Blue teams are both set to maxPlayers=10, minPlayers=5\. The Green team is set to maxPlayers=3, minPlayers=2\. Here's the sequence for filling this match: 

1. No team has reached minPlayers\. Red and Blue teams have 10 open slots, while Green has 3\. The first 10 players are assigned \(5 each\) to the Red and Blue teams\. Both teams have now reached minPlayers\.

1. Green team has not yet reached minPlayers\. The next 2 players are assigned to the Green team\.

1. All teams have now reached minPlayers\. Red and Blue teams have the most open slots, so the next 8 players are assigned \(4 each\) to the Red and Blue teams\.

1. Once all three teams have 1 open slot available, the remaining 3 player slots are assigned in no particular order\.

### Set Latency Rule for Large Matches<a name="match-design-rulesets-large-rule"></a>

Since most of the work of creating large matches is done using the balancing player attribute and prioritization strategy\. Most custom rules are not available\. However, you can create a rule that sets a hard limit on player latency\. 

To create this rule, use the `latency` rule type with the property *maxLatency*\. Here's an example that sets maximum player latency to 200 milliseconds:

```
"rules": [{
        "name": "player-latency",
        "type": "latency",
        "maxLatency": 200
    }],
```

### Relax Large Match Requirements<a name="match-design-rulesets-large-relax"></a>

As with small matches, you can use expansions to relax match requirements over time when no valid matches are possible\. With large matches, you have the option to relax either latency rules or team player counts\. 

If you're using automatic match backfill for large matches, avoid relaxing your team player counts too quickly\. FlexMatch starts generating backfill requests only after a game session starts, which may not happen for several seconds after a match is created\. During that time, FlexMatch can create multiple partially filled new game sessions, especially when the player count rules are lowered\. As a result, you end up with more game sessions than you need and players spread too thinly across them\. Best practice is to give the first step in your player count expansion a longer wait time, long enough for your game session to start\. Since backfill requests are given higher priority with large matches, incoming players will be slotted into existing games before new game are started\. You may need to experiment to find the ideal wait time for your game\.

Here's an example that gradually lowers the Yellow team's player count, with a longer initial wait time\. Keep in mind that wait times in rule set expansions are absolute, not compounded\. So the first expansion occurs at five seconds, and the second expansion occurs five seconds later, at ten seconds\.

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
```