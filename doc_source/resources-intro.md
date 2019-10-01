# Managing GameLift Hosting Resources<a name="resources-intro"></a>

The topics in this section detailed help with setting up and managing your game server hosting resources\. Whether you're using Amazon GameLift Realtime Servers or are deploying a fully custom game server, you will need to allocate resources, configure them, and manage ongoing scaling\.

**Tip**  
[Learn more about ways to explore Amazon GameLift features, including Realtime Servers, using sample games](gamelift-explore.md)\.

Here's a brief overview of the key terms and concepts for resource management with GameLift:

**Build**  
Your custom\-built game server software that runs on GameLift and hosts game sessions for your players\. A game build represents the set of files that run your game server on a particular operating system\. You can have many different builds, such as for different flavors of your game\. Once a build is integrated with GameLift, it is uploaded to the GameLift service\. 

**Script**  
Your configuration and custom game logic for use with Realtime Servers\. Realtime Servers are provided by GameLift to use in place of providing a custom game server\. You configure Realtime Servers for your game clients and add custom game logic as appropriate to host game sessions for your players\. A Realtime script is written in JavaScript\. It is uploaded to the GameLift service\.

**Fleet**  
Set of virtual hosting resources, called instances, that run your game servers and host game sessions for your players\. You create a fleet to deploy either your custom game build or a Realtime server script, and you configure how you want the fleet to operate for your game\. You set scaling rules to manage the fleet's size, which determines how many game sessionss and players it can accommodate\. All instances in a fleet operate in the region where you create the fleet\.

**Queue**  
List of destinations \(fleets or aliases\) where a requested game session may be placed\. A new game session request tells GameLift where to place the new game by specifying a fleet or a queue\. A queue, which can include fleets from different regions, offers more placement options and generally improves placement speed and efficiency\. A queue can also enforce limits on player latency to ensure that no games are placed where its players will experence slowed gameplay\. Queues are required for games that use FlexMatch or have fleets with spot instances\. 

**Alias**  
Nickname for fleet or endpoint\. Client requests for game sessions must specify a fleet \(or queue\) location\. By substituting an alias for a specific fleet ID and switching the alias destination as needed, you can switch the request location without having to update the game client\. 

**Matchmaking configuration**  
Set of instructions for how you want FlexMatch to process match requests\. These instructions are used to \(1\) find the best possible player matches, and \(2\) locate resources to host a game session for the match\. Also called a matchmaker, a configuration identifies a rule set for selecting players and a queue for locating resources\. You can have multiple different matchmaking configurations\. Each match request specifies which matchmaker to use\. 

**Matchmaking rule set**  
Set of instructions for how to build a match\. It defines the match's team structure and size, and specifies how to evaluate players for the best possible matches\. Although most rule sets are quite simple, the FlexMatch property expression language allows for very complex and precise rules\. A rule set is used by a matchmaking configuration\.