# How GameLift works<a name="gamelift-howitworks"></a>

This topic provides a general overview of the GameLift hosting solution\. The overview covers the core components for game hosting and describes how GameLift makes your multiplayer game servers available to players\.

Ready to prepare your game for hosting on GameLift? Check out [Getting started with Amazon GameLift](getting-started-intro.md)\.

## Key components<a name="gamelift-howitworks-components"></a>

Setting up GameLift to host your game involves working with the following components\. The diagram in [Game architecture with managed GameLift](gamelift-architecture.md) visualizes the relationships between these components\.
+ A **game server** is your game's server software running on a fleet\. You upload your **game server build** or **script** to GameLift and tell GameLift\. When you use GameLift Anywhere or GameLift FleetIQ, you upload your game server build directly to your hardware\.
+ A **game session** is your game in progress with players\. You define the basic characteristics of a game session, such as its life span and number of players\. Players then connect to the game server to join a game session\.
+ A **game client** is your game's software running on a player's device\. A game client connects to a game server through backend services to join a game session, based on connection information that it receives from GameLift\.
+ **Backend services** are additional, custom services that handle tasks related to GameLift\. As a best practice, your backend services should handle all game client communication with GameLift\.

## Hosting game servers<a name="gamelift-howitworks-allocating"></a>

With GameLift, you can host your game servers in three different ways: Managed GameLift, GameLift FleetIQ, and GameLift Anywhere\. Managed GameLift and GameLift Anywhere both use GameLift fleets to manage your game servers\. For more information about GameLift FleetIQ, see [What is GameLift FleetIQ?](https://docs.aws.amazon.com/gamelift/latest/fleetiqguide/gsg-intro.html)

You can design a fleet to fit your game's needs\. For more information about designing a fleet, see [GameLift fleet design guide](fleets-design.md)\.

**Managed GameLift**  
With managed GameLift, you can host your game servers on GameLift virtual computing resources, called **instances**\. Set up your hosting resources by creating a **fleet** of instances and deploying them to run your game servers\.

**GameLift Anywhere**  
With GameLift Anywhere, you can host your game servers on hardware that you manage\. Set up your hosting resources by creating an Anywhere fleet that references your hardware\. You can use GameLift tools such as FlexMatch, queues, and monitoring with your Anywhere fleet\.

**Fleet aliases**  
An **alias** is a designation that you can transfer between fleets, making it a convenient way to have a generic fleet location\. You can use an alias to switch game clients from using one fleet to another without having to change your game client\. You can also create a terminal alias, which lets you point to content instead of connecting to a server\. You can use an alias to transfer between managed and Anywhere fleets\.

## Running game sessions<a name="gamelift-howitworks-placing"></a>

After you deploy your game server code to a fleet and GameLift launches game server processes on each instance, the fleet can host game sessions\. GameLift starts new game sessions when your game client service sends a placement request to the backend service or to GameLift\.

**Game session placement and the FleetIQ algorithm**  
Game session placement handles the task of selecting an available game server to host a new game session\. The key component for game session placement is the GameLift **game session queue**\.

You assign a game session queue a list of fleets, which determines where the queue can place game sessions\. As a best practice, your queue's fleets should vary in fleet type, location, and instance type\. This diversity gives the queue greater flexibility to make placements wherever they make most sense for players\. For more information about game session queues and how to design them for your game, see [Design a game session queue](queues-design.md)\.

Queues use an algorithm, called FleetIQ, to look for the best possible placement for each game session request\. The FleetIQ algorithm prioritizes the search for available game servers based on low player latency, hosting costs, geographical locations, or other fleet characteristics\. For more information about FleetIQ, see [How GameLift queues works](queues-intro.md#queues-design-fleetiq)\.

**Player connections to games**  
As part of the game session placement process, the queue prompts the selected game server to start a new game session\. The game server responds to the prompt and reports back to GameLift when it's ready to accept player connections\. GameLift then delivers connection information to the backend service or game client service, as an IP address or DNS name, and a port\. Your game clients use this information to connect directly to the game session and begin gameplay\.

## Scaling fleet capacity<a name="gamelift-howitworks-capacity"></a>

When a fleet is active and ready to host game sessions, you can adjust your fleet capacity to meet player demand\. The amount of capacity that you use determines the cost of hosting\. We recommend that you find a balance between all incoming players finding a game quickly and overspending on resources that sit idle\.

You scale a fleet by adjusting the number of instances in it\. By scaling instances, you increase or decrease the availability for game sessions and players\. For fleets with instances in multiple locations, you can adjust capacity by location instead of for the entire fleet\.

GameLift provides a highly effective auto scaling tool\. You can also opt to manually set fleet capacity\. For more information, see [Scaling GameLift hosting capacity](fleets-manage-capacity.md)\.

**Auto scaling**  
With auto scaling enabled, GameLift tracks a fleet's hosting metrics and determines when to add or remove instances based on guidelines that you define\. GameLift can adjust capacity directly in response to changes in player demand\. For information about improving cost efficiency with auto scaling, see [Amazon GameLift Pricing](http://aws.amazon.com/gamelift/pricing/)\.

GameLift provides two methods of auto scaling:
+ [Target\-based auto scaling](fleets-autoscaling-target.md)
+ [Auto scale with rule\-based policies](fleets-autoscaling-rule.md)

**Additional scaling features**
+ **Game session protection** – Prevent GameLift from ending game sessions that are hosting active players during a scale\-down event\.
+ **Scaling limits** – Control overall instance usage by setting minimum and maximum limits on the number of instances in a fleet\.
+ **Suspending auto scaling** – Suspend auto scaling at the fleet location level without changing or deleting your auto scaling policies\.
+ **Scaling metrics** – Track a fleet's history of capacity and scaling events\.

## Monitoring GameLift<a name="gamelift-howitworks-telemetry"></a>

When you have fleets up and running, GameLift collects a variety of information to help you monitor the performance of your deployed game servers\. You can use this information to optimize your use of resources, troubleshoot issues, and gain insight into how players are active in your games\. GameLift collects the following:
+ Fleet, location, game session, and player session details
+ Usage metrics
+ Server process health
+ Game session logs

For more information about monitoring in GameLift, see [Monitoring Amazon GameLift](monitoring-overview.md)\.

## Using other AWS resources<a name="gamelift-howitworks-vpc"></a>

Your game servers and applications can communicate with other AWS resources\. For example, you might use a set of web services for player authentication or social networking\. For your game servers to access AWS resources that your AWS account manages, explicitly allow GameLift to access your AWS resources\.

GameLift provides a couple of options for managing this type of access\. For more information, see [Communicate with other AWS resources from your fleets](gamelift-sdk-server-resources.md)\.