# Managing GameLift hosting resources<a name="resources-intro"></a>

This section provides detailed information on setting up GameLift managed resources to run your game servers and enable them to host game sessions for players\. Whether you're deploying a fully custom game server or working with Realtime Servers, you need to configure and deploy resources, scale capacity to meet player demand, and set up a way to locate available resources to host new game sessions\. 

**Tip**  
[Learn more about ways to explore Amazon GameLift features, including Realtime Servers, using sample games](gamelift-explore.md)\.

Here's a summary of process required to set up GameLift resources for managed hosting\. The topics in this section discuss how to design and build each resource\.

1. Start by uploading your game server code to the GameLift service in the AWS Regions where you plan to set up fleets\. Your code might be a custom game server build or a Realtime configuration **script**\.

1. Next, create a **fleet** of computing resources in one or more Regions\. For each fleet, you specify the build/script to deploy, choose the type of instances \(virtual computing machine\) to use, configure them to run your game servers, and select the location\(s\) to deploy them to\. When a fleet is created, the build/script is installed on each instance in the fleet, ready to host game sessions\. 

1. Adjust fleet capacity to host game sessions as needed to meet player demand\. Optionally, set up an auto\-scaling policy\.

1. Create an **alias** for each fleet\. Although this step is optional, it is a best practice to abstract fleet identifiers to maintain uninterrupted game server availability then upgrading game builds and fleets\.

1. With a set of fleets deployed, set up a game session placement mechanism to find available game servers to host new game sessions\. The most common method is to set up a **game session queue**\. A queue is able to search for available game servers across multiple locations, fleet types, and instance types\. It uses FleetIQ algorithms to select the best possible game server, based on a defined set of priorities, and starts a new game session\.