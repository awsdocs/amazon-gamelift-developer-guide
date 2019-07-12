# Build a FlexMatch Rule Set<a name="match-rulesets"></a>

Every FlexMatch matchmaker must have a rule set\. The rule set determines the two key elements of a match: your game's team structure and size, and how to group players together for the best possible match\. 

For example, a rule set might describe a match like this: Create a match with two teams of five players each, one team is the defenders and the other team the invaders\. A team can have novice and experienced players, but the average skill of the two teams must within 10 points\. If no match is made after 30 seconds, gradually relax the skill requirements\.

The topics in this section describe how design and build a matchmaking rule set\. When creating a rule set, you can use either the Amazon GameLift console or the AWS CLI\.

**Topics**
+ [Design a FlexMatch Rule Set](match-design-ruleset.md)
+ [Create Matchmaking Rule Sets](match-create-ruleset.md)
+ [FlexMatch Rule Set Examples](match-examples.md)
+ [FlexMatch Rules Language](match-rules-reference.md)