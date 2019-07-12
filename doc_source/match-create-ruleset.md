# Create Matchmaking Rule Sets<a name="match-create-ruleset"></a>

Manage matchmaking rule sets for your FlexMatch matchmakers\. Use either the Amazon GameLift console or the AWS Command Line Interface \(CLI\)\. Learn more about how FlexMatch matchmaking works in [How Amazon GameLift FlexMatch Works](gamelift-match.md)\. 

Once created, matchmaking rule sets cannot be changed, so we recommend checking the rule set syntax before creating the rule set\. Both the console and the AWS CLI provide a validation option\. There is a maximum limit on the number of rule sets you can have, so it's a good idea to delete unused rule sets\.

------
#### [ Console ]

**To create a rule set:**

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/](https://console.aws.amazon.com/gamelift/)\.

1. Switch to the region where you want to place your rule set\. Rule sets must be defined in the same region as the matchmaking configuration they will be used with\.

1. From the Amazon GameLift main menu, choose **Create matchmaking rule set** and fill in the rule set details\.
   + **Rule set name** – Create a meaningful name so you can easily identify it in a list and in events and metrics\. The rule set name must be unique within a region\. Matchmaking configurations identify which rule set to use by its name\. Note: This is not the same as the "name" field in the rule set body, which is not currently used\.
   + **Rule set** – Enter the JSON text of a rule set body\. Learn more about designing a rule set in [Design a FlexMatch Rule Set](match-design-ruleset.md), or use one of the example rule sets from [FlexMatch Rule Set Examples](match-examples.md)\. 

1. Since rule sets can't be edited once they're created, it's a good idea to validate your rule set first\. Click **Validate rule set** to verify that the syntax of your rule set body is correct\. 

1. Once you've finished configuring a matchmaker, click **Create rule set**\. If creation is successful, the rule set can be used by a matchmaker\.<a name="match-delete-ruleset"></a>

**To delete a rule set:**

1. On the [Matchmaking rule sets](https://console.aws.amazon.com/gamelift/home#/r/matchmaking-rule-sets) console page, select a rule set and Click **Delete rule set**\.

1. If the rule set being deleted is currently being used by a matchmaking configuration, an error message is displayed\. In this case, you must change the matchmaking configuration to use a different rule set before you can delete the rule set\. To find out which matchmaking configurations are currently using a rule set, click on the rule set name to view the rule set's detail page\.

------
#### [ AWS CLI ]

**To create a rule set:**
+ To create a matchmaking rule set with the AWS CLI, open a command line window and use the command `create-matchmaking-rule-set` \([AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-matchmaking-rule-set.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

  This example creates a simple matchmaking rule set that sets up a single team\. Be sure to create the rule set in the same region as the matchmaking configurations that will reference it\.

  ```
  $ aws gamelift create-matchmaking-rule-set
  --name "SampleRuleSet123"
  --rule-set-body '{"name":  "aliens_vs_cowboys",
       "ruleLanguageVersion":  "1.0",
       "teams": [{
           "name":  "cowboys",
           "maxPlayers":  8,
           "minPlayers":  4}]}'
  ```

  *Copiable version:*

  ```
  aws gamelift create-matchmaking-rule-set --name "SampleRuleSet123" --rule-set-body '{"name": "aliens_vs_cowboys", "ruleLanguageVersion": "1.0", "teams": [{"name": "cowboys", "maxPlayers": 8, "minPlayers":  4}]}'
  ```

  If the creation request is successful, Amazon GameLift returns a [MatchmakingRuleSet](https://docs.aws.amazon.com/gamelift/latest/apireference/API_MatchmakingRuleSet.html) object that includes the settings you specified\. The new rule set can now be used by a matchmaker\. <a name="match-delete-ruleset-cli"></a>

**To delete a rule set:**
+ To delete a matchmaking rule set with the AWS CLI, open a command line window and use the command `delete-matchmaking-rule-set` \([AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/delete-matchmaking-rule-set.html)\)\.

  If the rule set being deleted is currently being used by a matchmaking configuration, an error message is displayed\. In this case, you must change the matchmaking configuration to use a different rule set before you can delete the rule set\. To get a list of which matchmaking configurations are currently using a rule set, use the command `describe-matchmaking-configurations` \([AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-matchmaking-configurations.html)\) and specify the rule set name\.

  This example first checks for the matchmaking rule set's usage and then deletes the rule set\.

  ```
  $ aws gamelift describe-matchmaking-configurations
  --rule-set-name "SampleRuleSet123"
  --limit 10
  
  $ aws gamelift delete-matchmaking-rule-set
  --name  "SampleRuleSet123"
  ```

  *Copiable versions:*

  ```
  aws gamelift describe-matchmaking-configurations --rule-set-name "SampleRuleSet123" --limit 10
  ```

  ```
  aws gamelift delete-matchmaking-rule-set --name "SampleRuleSet123"
  ```

  If the delete request is successful, Amazon GameLift returns success\. 

------