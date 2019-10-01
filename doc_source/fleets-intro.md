# Setting Up Amazon GameLift Fleets<a name="fleets-intro"></a>

This section provides detailed help with designing, building, and managing your fleets\. Start with [Design a Amazon GameLift Fleet for Your Game](fleets-design.md) to learn about the various options for creating a fleet\. 

Fleets are your hosting resources in the form of a set of EC2 instances\. To host game servers on Amazon GameLift, deploy a fleet with either a custom game server or Realtime Servers\. The size of a fleet depends on the number of instances you give it, and you can adjust fleet size to meet player demand either manually or by auto\-scaling\. 

Most games in production require more than one fleet\. You need multiple fleets if, for example, you want to host players in more than one region, or if you have two or more versions of your game server build or script \(such as free and premium versions\)\. 

**Tip**  
[Learn more about ways to explore Amazon GameLift features, including Realtime Servers, using sample games](gamelift-explore.md)\.

Once you have a fleet up and running, you can create a [fleet alias](aliases-creating.md), add the fleet to a [game session queue](queues-intro.md), and [manage fleet capacity](fleets-manage-capacity.md)\.

**Topics**
+ [Design a Amazon GameLift Fleet for Your Game](fleets-design.md)
+ [Deploy a GameLift Fleet for a Custom Game Build](fleets-creating.md)
+ [Deploy a Realtime Servers Fleet](realtime-fleets-creating.md)
+ [Manage Fleet Records](fleets-editing.md)
+ [Add an Alias to a Amazon GameLift Fleet](aliases-creating.md)
+ [Debug Fleet Issues](fleets-creating-debug.md)
+ [Remotely Access Fleet Instances](fleets-remote-access.md)