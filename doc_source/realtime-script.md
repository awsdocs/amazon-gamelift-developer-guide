# Creating a Realtime Script<a name="realtime-script"></a>

To use Realtime Servers for your game, you need to provide a script \(in the form of some JavaScript code\) to configure and optionally customize a fleet of Realtime Servers\. This topic covers the key steps in creating a Realtime script\. Once the script is ready, upload it to the Amazon GameLift service and use it to create a fleet \(see [Upload a Realtime Servers Script to Amazon GameLift](realtime-script-uploading.md)\)\.

To prepare a script for use with Realtime Servers, add the following functionality to your Realtime script\.

## Manage Game Session Life\-Cycle \(required\)<a name="realtime-script-init"></a>

At a minimum, a Realtime script must include the `Init()` function, which prepares the Realtime server to start a game session\. It is also highly recommended that you also provide a way to terminate game sessions, to ensure that new game sessions can continue to be started on your fleet\.

The `Init()` callback function, when called, is passed a Realtime session object, which contains an interface for the Realtime server\. See [Realtime Servers Interface](realtime-script-objects.md) for more details on this interface\.

To gracefully end a game session, the script must also call the Realtime server's `session.processEnding` function\. This requires some mechanism to determine when to end a session\. The script example code illustrates a simple mechanism that checks for player connections and triggers game session termination when no players have been connected to the session for a specified length of time\.

Realtime Servers with the most basic configuration\-\-server process initialization and termination\-\-essentially act as stateless relay servers\. The Realtime server relays messages and game data between game clients that are connected to the game, but takes no independent action to process data or perform logic\. You can optionally add game logic, triggered by game events or other mechanisms, as needed for your game\.

## Add Server\-Side Game Logic \(optional\)<a name="realtime-script-logic"></a>

You can optionally add game logic to your Realtime script\. For example, you might do any or all of the following\. The script example code provides illustration\. See [Amazon GameLift Realtime Servers Script Reference](realtime-script-ref.md)\. 
+ **Add event\-driven logic\.** Implement the callback functions to respond to client\-server events\. See [ Script Callbacks for Realtime ServersScript Callbacks  Look up options callback methods that can be implement in the script for Realtime Servers   You can provide custom logic to respond to events by implementing these callbacks in your Realtime script\.   init  Initializes the Realtime server and receives a Realtime server interface\.   Syntax 

```
init(rtsession)
```     onMessage  Invoked when a received message is sent to the server\.   Syntax 

```
onMessage(gameMessage)
```      onHealthCheck  Invoked to set the status of the game session health\. By default, health status is healthy \(or `true`\. This callback can be implemented to perform custom health checks and return a status\.  Syntax 

```
onHealthCheck()
```    onStartGameSession  Invoked when a new game session starts, with a game session object passed in\.  Syntax 

```
onStartGameSession(session)
```    onProcessTerminate  Invoked when the server process is being terminated by the Amazon GameLift service\. This can act as a trigger to exit cleanly from the game session\. There is no need to call `processEnding().`  Syntax 

```
onProcessTerminate()
```    onPlayerConnect  Invoked when a player requests a connection and has passed initial validation\.   Syntax 

```
onPlayerConnect(connectMessage)
```    onPlayerAccepted  Invoked when a player connection is accepted\.  Syntax 

```
onPlayerAccepted(player)
```    onPlayerDisconnect  Invoked when a player disconnects from the game session, either by sending a disconnect request or by other means\.  Syntax 

```
onPlayerDisconnect(peerId)
```    onProcessStarted  Invoked when a server process is started\. This callback allows the script to perform any custom tasks needed to prepare to host a game session\.   Syntax 

```
onProcessStarted(args)
```    onSendToPlayer  Invoked when a message is received on the server from one player to be delivered to another player\. This process runs before the message is delivered\.   Syntax 

```
nSendToPlayer(gameMessage)
```    onSendToGroup  Invoked when a message is received on the server from one player to be delivered to a group\. This process runs before the message is delivered\.   Syntax 

```
onSendToGroup(gameMessage))
```    onPlayerJoinGroup  Invoked when a player sends a request to join a group\.  Syntax 

```
onPlayerJoinGroup(groupId, peerId)
```    onPlayerLeaveGroup  Invoked when a player sends a request to leave a group\.  Syntax 

```
onPlayerLeaveGroup(groupId, peerId)
```  ](realtime-script-callbacks.md) for a complete list of callbacks\.
+ **Trigger logic by sending messages to the server\.** Create a set of special operation codes for messages sent from game clients to the server, and add functions to handle receipt\. Use the callback `onMessage`, and parse the message content using the `gameMessage` interface \(see [gameMessage\.opcode](realtime-script-objects.md#realtime-script-objects-gamemessageopcode)\)\. 

## Realtime Servers Script Example<a name="realtime-script-examples"></a>

### <a name="realtime-script-examples-custom"></a>

This example illustrates a basic script needed to deploy Realtime Servers plus some custom logic\. It contains the required `Init()` function, and uses a timer mechanism to trigger game session termination based on length of time with no player connections\. It also includes some hooks for custom logic, including some callback implementations\. 

```
// Example Realtime Server Script
'use strict';

// Example override configuration
const configuration = {
    pingIntervalTime: 30000
};

// Timing mechanism used to trigger end of game session. Defines how long, in milliseconds, between each tick in the example tick loop
const tickTime = 1000;

// Defines how to long to wait in Seconds before beginning early termination check in the example tick loop
const minimumElapsedTime = 120;

var session;                        // The Realtime server session object
var logger;                         // Log at appropriate level via .info(), .warn(), .error(), .debug()
var startTime;                      // Records the time the process started
var activePlayers = 0;              // Records the number of connected players
var onProcessStartedCalled = false; // Record if onProcessStarted has been called

// Example custom op codes for user-defined messages
// Any positive op code number can be defined here. These should match your client code.
const OP_CODE_CUSTOM_OP1 = 111;
const OP_CODE_CUSTOM_OP1_REPLY = 112;
const OP_CODE_PLAYER_ACCEPTED = 113;
const OP_CODE_DISCONNECT_NOTIFICATION = 114;

// Example groups for user defined groups
// Any positive group number can be defined here. These should match your client code.
const RED_TEAM_GROUP = 1;
const BLUE_TEAM_GROUP = 2;

// Called when game server is initialized, passed server's object of current session
function init(rtSession) {
    session = rtSession;
    logger = session.getLogger();
}

// On Process Started is called when the process has begun and we need to perform any
// bootstrapping.  This is where the developer should insert any code to prepare
// the process to be able to host a game session, for example load some settings or set state
//
// Return true if the process has been appropriately prepared and it is okay to invoke the
// GameLift ProcessReady() call.
function onProcessStarted(args) {
    onProcessStartedCalled = true;
    logger.info("Starting process with args: " + args);
    logger.info("Ready to host games...");

    return true;
}

// Called when a new game session is started on the process
function onStartGameSession(gameSession) {
    // Complete any game session set-up

    // Set up an example tick loop to perform server initiated actions
    startTime = getTimeInS();
    tickLoop();
}

// Handle process termination if the process is being terminated by GameLift
// You do not need to call ProcessEnding here
function onProcessTerminate() {
    // Perform any clean up
}

// Return true if the process is healthy
function onHealthCheck() {
    return true;
}

// On Player Connect is called when a player has passed initial validation
// Return true if player should connect, false to reject
function onPlayerConnect(connectMsg) {
    // Perform any validation needed for connectMsg.payload, connectMsg.peerId
    return true;
}

// Called when a Player is accepted into the game
function onPlayerAccepted(player) {
    // This player was accepted -- let's send them a message
    const msg = session.newTextGameMessage(OP_CODE_PLAYER_ACCEPTED, player.peerId,
                                             "Peer " + player.peerId + " accepted");
    session.sendReliableMessage(msg, player.peerId);
    activePlayers++;
}

// On Player Disconnect is called when a player has left or been forcibly terminated
// Is only called for players that actually connected to the server and not those rejected by validation
// This is called before the player is removed from the player list
function onPlayerDisconnect(peerId) {
    // send a message to each remaining player letting them know about the disconnect
    const outMessage = session.newTextGameMessage(OP_CODE_DISCONNECT_NOTIFICATION,
                                                session.getServerId(),
                                                "Peer " + peerId + " disconnected");
    session.getPlayers().forEach((player, playerId) => {
        if (playerId != peerId) {
            session.sendReliableMessage(outMessage, peerId);
        }
    });
    activePlayers--;
}

// Handle a message to the server
function onMessage(gameMessage) {
    switch (gameMessage.opCode) {
      case OP_CODE_CUSTOM_OP1: {
        // do operation 1 with gameMessage.payload for example sendToGroup
        const outMessage = session.newTextGameMessage(OP_CODE_CUSTOM_OP1_REPLY, session.getServerId(), gameMessage.payload);
        session.sendGroupMessage(outMessage, RED_TEAM_GROUP);
        break;
      }
    }
}

// Return true if the send should be allowed
function onSendToPlayer(gameMessage) {
    // This example rejects any payloads containing "Reject"
    return (!gameMessage.getPayloadAsText().includes("Reject"));
}

// Return true if the send to group should be allowed
// Use gameMessage.getPayloadAsText() to get the message contents
function onSendToGroup(gameMessage) {
    return true;
}

// Return true if the player is allowed to join the group
function onPlayerJoinGroup(groupId, peerId) {
    return true;
}

// Return true if the player is allowed to leave the group
function onPlayerLeaveGroup(groupId, peerId) {
    return true;
}

// A simple tick loop example
// Checks to see if a minimum amount of time has passed before seeing if the game has ended
async function tickLoop() {
    const elapsedTime = getTimeInS() - startTime;
    logger.info("Tick... " + elapsedTime + " activePlayers: " + activePlayers);

    // In Tick loop - see if all players have left early after a minimum period of time has passed
    // Call processEnding() to terminate the process and quit
    if ( (activePlayers == 0) && (elapsedTime > minimumElapsedTime)) {
        logger.info("All players disconnected. Ending game");
        const outcome = await session.processEnding();
        logger.info("Completed process ending with: " + outcome);
        process.exit(0);
    }
    else {
        setTimeout(tickLoop, tickTime);
    }
}

// Calculates the current time in seconds
function getTimeInS() {
    return Math.round(new Date().getTime()/1000);
}

exports.ssExports = {
    configuration: configuration,
    init: init,
    onProcessStarted: onProcessStarted,
    onMessage: onMessage,
    onPlayerConnect: onPlayerConnect,
    onPlayerAccepted: onPlayerAccepted,
    onPlayerDisconnect: onPlayerDisconnect,
    onSendToPlayer: onSendToPlayer,
    onSendToGroup: onSendToGroup,
    onPlayerJoinGroup: onPlayerJoinGroup,
    onPlayerLeaveGroup: onPlayerLeaveGroup,
    onStartGameSession: onStartGameSession,
    onProcessTerminate: onProcessTerminate,
    onHealthCheck: onHealthCheck
};
```