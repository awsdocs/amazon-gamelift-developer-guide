# Setting up GameLift fleets<a name="fleets-intro"></a>

This section provides detailed help with designing, building, and maintaining fleets for use with a GameLift managed hosting solution\. GameLift managed fleets are used to deploy custom game servers and Realtime Servers\. 

A fleet represents your hosting resources in the form of a set of EC2 virtual computing machines, called instances\. A fleet's locations determines where, geographically, instances are deployed to host game sessions for your players\. The size of a fleet, and the number of game sessions and players it can support, depends on the number of instances you give it, which you can adjust either manually or by auto\-scaling\. 

Many games in production use more than one fleet\. You need multiple fleets to, for example, have more than one version of your game server running simultaneously, provide back\-up capacity for Spot fleets, or to build in redundancy\. 

 To learn about how to create fleets that best suit your game needs, start with [GameLift fleet design guide](fleets-design.md)\. Once you get a fleet up and running, you can [manage fleet capacity](fleets-manage-capacity.md), create a [fleet alias](aliases-creating.md), and add the fleet to a [game session queue](queues-intro.md)\.

**Topics**
+ [GameLift fleet design guide](fleets-design.md)
+ [Create a new GameLift fleet](fleets-creating-all.md)
+ [Manage your GameLift fleets](fleets-editing.md)
+ [Add an alias to a GameLift fleet](aliases-creating.md)
+ [Debug GameLift fleet issues](fleets-creating-debug.md)
+ [Remotely access GameLift fleet instances](fleets-remote-access.md)