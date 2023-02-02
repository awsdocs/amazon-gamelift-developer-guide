# GameLift managed hosting roadmap<a name="gamelift-quickstart-intro"></a>

This topic helps you choose from the different Amazon GameLift hosting options for your session\-based multiplayer game\. The rest of the topics in this section walk you through how to use GameLift for your managed hosting\.

Before you start preparing to launch your game to production, fill out the launch questionnaire to begin working with the GameLift team\.

**Topics**
+ [Choose a hosting option](#gamelift-quickstart-intro-deciding)
+ [Prepare your game for GameLift](gamelift_quickstart_integration.md)
+ [Test your integration with GameLift](gamelift_quickstart_test.md)
+ [Plan and deploy your GameLift resources](gamelift_quickstart_plan.md)
+ [Design your backend service](gamelift_quickstart_customservers_designbackend.md)
+ [Set up metrics and logging for GameLift](gamelift_quickstart_metrics.md)
+ [Game launch checklists](gamelift_quickstart_customservers_checklist.md)

## Choose a hosting option<a name="gamelift-quickstart-intro-deciding"></a>

The following flowchart asks questions to lead you to the correct GameLift solution for your use case\. 

1. Do you want a managed solution for game server management?
   + **Yes** – Continue to step two\.
   + **No** – Consider self\-managed game servers on Amazon EC2 instances\.

1. Do you need full control of the instances hosting your game servers?
   + **Yes** – Consider GameLift FleetIQ\.
   + **No** – Continue to step 3\.

1. Do you have existing infrastructure you want to use with GameLift?
   + **Yes** – Consider GameLiftAnywhere\.
   + **No** – Continue to step four\.

1. Is your game lightweight without existing game server logic?
   + **Yes** – Consider Realtime servers\.
   + **No** – Consider custom servers\.

![\[A flowchart with Yes/No questions that help you choose a GameLift game server hosting option.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_decision_tree.png)

Here's some more information about some of the GameLift hosting options mentioned in the flowchart:

**GameLift Anywhere**  
Use GameLift Anywhere to host your games on your own hardware with the benefit of GameLift management tools\. You can also use Anywhere fleets to test your game servers iteratively\. For more information, see [Create a GameLift Anywhere fleet](fleets-creating-anywhere.md)\.

**Managed GameLift**  
There are two options for managed GameLift hosting:  
**Custom servers** – GameLift hosts your custom server that runs your game server binary\.  
**Realtime Servers** – GameLift hosts your lightweight game server\.

**GameLift FleetIQ**  
In the flowchart, a *lift and shift* migration refers to a migration when you can't make changes to the game architecture\. Using GameLift FleetIQ requires fewer changes to your existing deployment and provides GameLift tools for fleet management\. For more information, see the [Amazon GameLift FleetIQ Developer Guide](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide)\.

If you decide to use GameLift Anywhere or managed GameLift, continue to [Prepare your game for GameLift](gamelift_quickstart_integration.md)\.