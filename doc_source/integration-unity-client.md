# Add Amazon GameLift to your Unity game client project<a name="integration-unity-client"></a>

This topic helps you set up a game client to connect to GameLift hosted game sessions through a backend service\. Use GameLift APIs to initiate matchmaking, request game session placement, and more\. 

Add code to the backend service project to allow communication with the GameLift service\. A backend service handles all game client communication with the GameLift service\. For more information about backend services, see [Design your backend service](gamelift_quickstart_customservers_designbackend.md)\.

 A backend server handles the following game client tasks: 
+ Customize authentication for your players\. 
+ Request information about active game sessions from the GameLift service\.
+ Create a new game session\.
+ Add a player to an existing game session\.\.
+ Remove a player from an existing game session\.

**Topics**
+ [Prerequisites](#integration-unity-client-prereq)
+ [Initialize a game client](#integration-unity-client-initialize)
+ [Create game session on a specific fleet](#integration-unity-client-game-session)
+ [Add players to game sessions](#integration-unity-client-add-player)
+ [Remove a player from a game session](#integration-unity-client-remove-player)

## Prerequisites<a name="integration-unity-client-prereq"></a>

Before setting up game server communication with the GameLift client, complete the following tasks: 
+ [Set up an AWS account](setting-up-aws-login.md)
+ [Installing and setting up the plug\-in for Unity](unity-plug-in-install.md)
+ [Add Amazon GameLift to your Unity game server project](integration-unity-server.md)
+ [Setting up GameLift fleets](fleets-intro.md)

## Initialize a game client<a name="integration-unity-client-initialize"></a>

Add code to initialize a game client\. Run this code on launch, it's necessary for other GameLift functions\. 

1. Initialize `AmazonGameLiftClient`\. Call `AmazonGameLiftClient` with either a default client configuration or a custom configuration\. For more information on how to configure a client, see [Set up GameLift on a backend service](gamelift-sdk-client-api.md#gamelift-sdk-client-api-initialize)\. 

1. Generate a unique player id for each player to connect to a game session\. For more information see [Generate player IDs](player-sessions-player-identifiers.md)\. 

   The following examples shows how to set up a GameLift client\. 

   ```
   public class GameLiftClient
   {
       private GameLift gl;
       //A sample way to generate random player IDs. 
       bool includeBrackets = false;
       bool includeDashes = true;
       string playerId = AZ::Uuid::CreateRandom().ToString<string>(includeBrackets, includeDashes);
   		
       
       private Amazon.GameLift.Model.PlayerSession psession = null;
       public AmazonGameLiftClient aglc = null;
   
       public void CreateGameLiftClient()
       {
           //Access Amazon GameLift service by setting up a configuration. 
           //The default configuration specifies a location. 
           var config = new AmazonGameLiftConfig();
           config.RegionEndpoint = Amazon.RegionEndpoint.USEast1;
      
           CredentialProfile profile = null;
           var nscf = new SharedCredentialsFile();
           nscf.TryGetProfile(profileName, out profile);
           AWSCredentials credentials = profile.GetAWSCredentials(null); 
           //Initialize GameLift Client with default client configuration.
           aglc = new AmazonGameLiftClient(credentials, config); 
           
       }
   }
   ```

## Create game session on a specific fleet<a name="integration-unity-client-game-session"></a>

Add code to start new game sessions on your deployed fleets and make them available to players\. After GameLift has created the new game session and returned a `GameSession`, you can add players to it\. 
+ Place a request for a new game session\. 
  + If your game uses fleets, call `CreateGameSession()` with a fleet or alias ID, a session name, and maximum number of concurrent players for the game\.
  + If your game uses queues, call `StartGameSessionPlacement()`\.

 The following example shows how to create a game session\. 

```
public Amazon.GameLift.Model.GameSession()
{
    var cgsreq = new Amazon.GameLift.Model.CreateGameSessionRequest();
    //A unique identifier for the alias with the fleet to create a game session in.
    cgsreq.AliasId = aliasId; 
    //A unique identifier for a player or entity creating the game session 
    cgsreq.CreatorId = playerId; 
    //The maximum number of players that can be connected simultaneously to the game session.
    cgsreq.MaximumPlayerSessionCount = 4; 
						
    //Prompt an available server process to start a game session and retrieves connection information for the new game session
    Amazon.GameLift.Model.CreateGameSessionResponse cgsres = aglc.CreateGameSession(cgsreq);
    string gsid = cgsres.GameSession != null ? cgsres.GameSession.GameSessionId : "N/A";
    Debug.Log((int)cgsres.HttpStatusCode + " GAME SESSION CREATED: " + gsid);
    return cgsres.GameSession;
}
```

## Add players to game sessions<a name="integration-unity-client-add-player"></a>

After GameLift has created the new game session and returned a `GameSession` object, you can add players to it\.

1. Reserve a player slot in a game session by creating a new player session\. Use `CreatePlayerSession` or `CreatePlayerSessions` with the game session ID and a unique ID for each player\.

1. Connect to the game session\. Retrieve the `PlayerSession` object to get the game session's connection information\. You can use this information to establish a direct connection to the server process: 

   1. Use the specified port and either the DNS name or IP address of the server process\.

   1. Use the DNS name and port of your fleets\. The DNS name and port are required if your fleets have TLS certificate generation enabled\. 

   1. Reference the player session ID\. The player session ID is required if your game server validates incoming player connections\.

The following examples demonstrates how to reserve a player spot in a game session\. 

```
public Amazon.GameLift.Model.PlayerSession CreatePlayerSession(Amazon.GameLift.Model.GameSession gsession)
{
    var cpsreq = new Amazon.GameLift.Model.CreatePlayerSessionRequest();
    cpsreq.GameSessionId = gsession.GameSessionId; 
    //Specify game session ID.
    cpsreq.PlayerId = playerId; 
    //Specify player ID.
    Amazon.GameLift.Model.CreatePlayerSessionResponse cpsres = aglc.CreatePlayerSession(cpsreq);
    string psid = cpsres.PlayerSession != null ? cpsres.PlayerSession.PlayerSessionId : "N/A";
    return cpsres.PlayerSession;  
}
```

The following code illustrates how to connect a player with the game session\.

```
public bool ConnectPlayer(int playerIdx, string playerSessionId)
{
    //Call ConnectPlayer with player ID and player session ID. 
    return server.ConnectPlayer(playerIdx, playerSessionId);
}
```

## Remove a player from a game session<a name="integration-unity-client-remove-player"></a>

You can remove the players from the game session when they leave the game\.

1. Notify the GameLift service that a player has disconnected from the server process\. Call `RemovePlayerSession` with the player's session ID\.

1. Verify that `RemovePlayerSession` returns `Success`\. Then, GameLift changes the player slot to be available, which GameLift can assign to a new player\. 

 The following example illustrates how to remove a player session\. 

```
public void DisconnectPlayer(int playerIdx)
{
    //Receive the player session ID. 
    string playerSessionId = playerSessions[playerIdx];
    var outcome = GameLiftServerAPI.RemovePlayerSession(playerSessionId);
    if (outcome.Success)
    {
        Debug.Log (":) PLAYER SESSION REMOVED");
    }
    else
    {
        Debug.Log(":(PLAYER SESSION REMOVE FAILED. RemovePlayerSession()
        returned " + outcome.Error.ToString());
    }
}
```