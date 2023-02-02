# Setting up GameLift fleets<a name="fleets-intro"></a>

This section provides detailed information about designing, building, and maintaining fleets for use with Amazon GameLift\. You can use GameLift fleets to deploy custom game servers and Realtime Servers\.

A fleet represents your hosting resources as a set of Amazon Elastic Compute Cloud \(Amazon EC2\) instances or physical hardware\. A fleet's location determines where instances or hardware are deployed to host game sessions for your players\. The size of a fleet, and the number of game sessions and players that it can support, depends on the number of instances or the amount of hardware that you give it\. You can adjust virtual instances manually or by using automatic scaling\.

Many games in production use more than one fleet\. You can use multiple fleets, for example, to have more than one version of your game server running simultaneously, to provide backup capacity for Spot Fleets, or to build in redundancy\.

To learn how to create fleets designed for your game's needs, start with [GameLift fleet design guide](fleets-design.md)\. After your fleet is running, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md), [Add an alias to a GameLift fleet](aliases-creating.md), and [Setting up GameLift queues for game session placement](queues-intro.md)\.

**Topics**
+ [GameLift fleet design guide](fleets-design.md)
+ [Create a new GameLift fleet](fleets-creating-all.md)
+ [Manage your GameLift fleets](fleets-editing.md)
+ [Add an alias to a GameLift fleet](aliases-creating.md)
+ [Debug GameLift fleet issues](fleets-creating-debug.md)
+ [Remotely access GameLift fleet instances](fleets-remote-access.md)