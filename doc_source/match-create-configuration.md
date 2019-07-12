# Create a Matchmaking Configuration<a name="match-create-configuration"></a>

To set up a FlexMatch matchmaker, you create a new matchmaking configuration\. Use either the Amazon GameLift console or the AWS Command Line Interface \(AWS CLI\)\. For more detailed information on configuring a matchmaker, see [Design a FlexMatch Matchmaker](match-configuration.md)\. 
<a name="match-create-configuration-placing"></a>
**Choose a Region for the Matchmaker**  
A matchmaker is hosted in the region where its configuration is created\. It's worth considering how the location of a matchmaker might affect its performance and how to refine the match experience for its intended players\. We recommend that you place a matchmaker in a region close to the clients or client service that will be requesting matchmaking\. As a best practice, we also recommend putting the matchmaker and the queue it uses in the same region\. This helps to minimize communication latency between the matchmaker and queue\. 
<a name="match-create-configuration-console"></a>
**Create a Matchmaker**  
Before you can create a matchmaking configuration, you must create the [rule set](https://docs.aws.amazon.com/gamelift/latest/developerguide/match-create-ruleset.html) and [queue](https://docs.aws.amazon.com/gamelift/latest/developerguide/queues-creating.html) that you want to use with the matchmaker\. 

------
#### [ Console ]

1. Open the Amazon GameLift console at [https://console\.aws\.amazon\.com/gamelift/home](https://console.aws.amazon.com/gamelift/)\.

1. Switch to the region where you want to place your matchmaker\.

1. From the main menu, choose **Create matchmaking configuration**\. Fill in the matchmaking configuration details\.
   + **Name** – Create a meaningful matchmaker name so you can easily identify it in a list and in metrics\. The matchmaker name must be unique within a region\. Matchmaking requests identify which matchmaker to use by its name and region\.
   + **Description** – \(Optional\) Add a description of the matchmaker\. The description is used for identification only; it is not used in the matchmaking process\.
   + **Queue** – Choose the game session queue to use with this matchmaker\. To find the queue, first choose the region where the queue is configured\. Then choose the queue you want from the list of available queues in that region\.
   + **Request timeout** – Type the maximum amount of time, in seconds, for the matchmaker to complete a match for each request\. Matchmaking requests that exceed this time are terminated\.
   + **Acceptance required** – \(Optional\) Indicate whether to require each player in a proposed match to actively accept participation in the match\. If you chose yes, indicate how long you want the matchmaker to wait for player acceptances before canceling the match\. 
   + **Rule set name** – Choose the rule set to use with this matchmaker\. The list contains all rule sets that have been created in the current region\. 
   + **Backfill mode** – Specify a method for handling match backfills\. Select "Automatic" to turn on the automatic backfill feature\. Or select "Manual" if you are managing backfill requests in your game server or game client, or if you opt to not backfill your games\. 
   + **Notification target** – \(Optional\) Type the ARN of an SNS topic for receiving matchmaking event notifications\. If you haven't set one up yet, you can add this information later by editing the matchmaking configuration\. See [Set up FlexMatch Event Notification](match-notification.md)
   + **Additional players** – \(Optional\) Specify the number of player slots to remain unfilled in each new match\. These slots can be filled with players in the future\.
   + **Custom event data** – \(Optional\) Specify any data you want to associate with this matchmaker in event messaging\. This data is included in every event that is associated with the matchmaker\.

1. Once you've finished configuring a matchmaker, click **Create**\. If the creation is successful, the matchmaker is immediately ready to accept matchmaking requests\. 

------
#### [ AWS CLI ]

To create a matchmaking configuration with the AWS CLI, open a command line window and use the `create-matchmaking-configuration` command to define a new matchmaker\. See complete documentation on this command in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/create-matchmaking-configuration.html)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/)

This example creates a new matchmaking configuration that requires player acceptance and uses notifications to track the status of matchmaking requests\. It also reserves two player slots for additional players to be added later\.

```
$ aws gamelift create-matchmaking-configuration
--name "SampleMatchamker123"
--description "The sample test matchmaker with acceptance"
--game-session-queue-arns "arn:aws:gamelift:us-west-2:111122223333:gamesessionqueue/My_Game_Session_Queue_One"
--rule-set-name "My_Rule_Set_One"
--request-timeout-seconds "120"
--acceptance-required "true"
--acceptance-timeout-seconds "30"
--backfill-mode "AUTOMATIC"
--notification-target "arn:aws:sns:us-west-2:111122223333:My_Matchmaking_SNS_Topic"
--additional-player-count "2"
--game-session-data "key=map,value=winter444"
```

*Copiable version:*

```
aws gamelift create-matchmaking-configuration --name "SampleMatchamker123" --description "The sample test matchmaker with acceptance" --game-session-queue-arns "arn:aws:gamelift:us-west-2:111122223333:gamesessionqueue/My_Game_Session_Queue_One" --rule-set-name "My_Rule_Set_One" --request-timeout-seconds "120" --acceptance-required "true" --acceptance-timeout-seconds "30" --backfill-mode "AUTOMATIC" --notification-target "arn:aws:sns:us-west-2:111122223333:My_Matchmaking_SNS_Topic" --additional-player-count "2" --game-session-data "key=map,value=winter444"
```

If the matchmaking configuration creation request is successful, Amazon GameLift returns a [MatchmakingConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_MatchmakingConfiguration.html) object with the settings that you requested for the matchmaker\. The new matchmaker is immediately ready to accept matchmaking requests\.

------