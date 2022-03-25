# Realtime Servers interface<a name="realtime-script-objects"></a>

When a Realtime script initializes, an interface to the Realtime server is returned\. This topic describes the properties and methods available through the interface\. Learn more about writing Realtime scripts and view a detailed script example in [Creating a Realtime script](realtime-script.md)\.

The Realtime interface provides access to the following objects:
+ session
+ player
+ gameMessage
+ configuration

**Realtime Session object**  
Use these methods to access server\-related information and perform server\-related actions\.

## getPlayers\(\)<a name="realtime-script-objects-getplayers"></a>

Retrieves a list of peer IDs for players that are currently connected to the game session\. Returns an array of player objects\. 

### Syntax<a name="realtime-script-objects-getplayers-syntax"></a>

```
rtSession.getPlayers()
```

## broadcastGroupMembershipUpdate\(\)<a name="realtime-script-objects-broadcastgroup"></a>

Triggers delivery of an updated group membership list to player group\. Specify which membership to broadcast \(groupIdToBroadcast\) and the group to receive the update \(targetGroupId\)\. Group IDs must be a positive integer or "\-1" to indicate all groups\. See [Realtime Servers script example](realtime-script.md#realtime-script-examples) for an example of user\-defined group IDs\.

### Syntax<a name="realtime-script-objects-broadcastgroup-syntax"></a>

```
rtSession.broadcastGroupMembershipUpdate(groupIdToBroadcast, targetGroupId)
```

## getServerId\(\)<a name="realtime-script-objects-getserverid"></a>

Retrieves the server's unique peer ID identifier, which is used to route messages to the server\. 

### Syntax<a name="realtime-script-objects-getserverid-syntax"></a>

```
rtSession.getServerId()
```

## getAllPlayersGroupId\(\)<a name="realtime-script-objects-getallplayersgroupid"></a>

Retrieves the group ID for the default group that contains all players currently connected to the game session\.

### Syntax<a name="realtime-script-objects-getallplayersgroupid-syntax"></a>

```
rtSession.getAllPlayersGroupId()
```

## processEnding\(\)<a name="realtime-script-objects-processending"></a>

Triggers the Realtime server to terminate the game server\. This function must be called from the Realtime script to exit cleanly from a game session\.

### Syntax<a name="realtime-script-objects-processending-syntax"></a>

```
rtSession.processEnding()
```

## getGameSessionId\(\)<a name="realtime-script-objects-getgamesessionid"></a>

Retrieves the unique ID of the game session currently running\.

### Syntax<a name="realtime-script-objects-getgamesessionid-syntax"></a>

```
rtSession.getGameSessionId()
```

## getLogger\(\)<a name="realtime-script-objects-getlogger"></a>

Retrieves the interface for logging\. Use this to log statements that will be captured in your game session logs\. The logger supports use of "info", "warn", and "error" statements\. For example: `logger.info("<string>")`\.

### Syntax<a name="realtime-script-objects-getlogger-syntax"></a>

```
rtSession.getLogger()
```

## sendMessage\(\)<a name="realtime-script-objects-sendmessage"></a>

Sends a message, created using `newTextGameMessage` or `newBinaryGameMessage`, from the Realtime server to a player recipient using the UDP channel\. Identify the recipient using the player's peer ID\.

### Syntax<a name="realtime-script-objects-sendmessage-syntax"></a>

```
rtSession.sendMessage(gameMessage, targetPlayer)
```

## sendGroupMessage\(\)<a name="realtime-script-objects-sendgroupmessage"></a>

Sends a message, created using `newTextGameMessage` or `newBinaryGameMessage`, from the Realtime server to all players in a player group using the UDP channel\. Group IDs must be a positive integer or "\-1" to indicate all groups\. See [Realtime Servers script example](realtime-script.md#realtime-script-examples) for an example of user\-defined group IDs\.

### Syntax<a name="realtime-script-objects-sendgroupmessage-syntax"></a>

```
rtSession.sendGroupMessage(gameMessage, targetGroup)
```

## sendReliableMessage\(\)<a name="realtime-script-objects-sendreliablemessage"></a>

Sends a message, created using `newTextGameMessage` or `newBinaryGameMessage`, from the Realtime server to a player recipient using the TCP channel\. Identify the recipient using the player's peer ID\.

### Syntax<a name="realtime-script-objects-sendreliablemessage-syntax"></a>

```
rtSession.sendReliableMessage(gameMessage, targetPlayer)
```

## sendReliableGroupMessage\(\)<a name="realtime-script-objects-sendreliablegroupmessage"></a>

Sends a message, created using `newTextGameMessage` or `newBinaryGameMessage`, from the Realtime server to all players in a player group using the TCP channel\. Group IDs which must be a positive integer or "\-1" to indicate all groups\. See [Realtime Servers script example](realtime-script.md#realtime-script-examples) for an example of user\-defined group IDs\.

### Syntax<a name="realtime-script-objects-sendreliablegroupmessage-syntax"></a>

```
rtSession.sendReliableGroupMessage(gameMessage, targetGroup)
```

## newTextGameMessage\(\)<a name="realtime-script-objects-newtextgamemessage"></a>

Creates a new message containing text, to be sent from the server to player recipients using the SendMessage functions\. Message format is similar to the format used in the Realtime Client SDK \(see [RTMessage](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-rtmessage)\)\. Returns a `gameMessage` object\.

### Syntax<a name="realtime-script-objects-newtextgamemessage-syntax"></a>

```
rtSession.newTextGameMessage(opcode, sender, payload)
```

## newBinaryGameMessage\(\)<a name="realtime-script-objects-newbinarygamemessage"></a>

Creates a new message containing binary data, to be sent from the server to player recipients using the SendMessage functions\. Message format is similar to the format used in the Realtime Client SDK \(see [RTMessage](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-rtmessage)\)\. Returns a `gameMessage` object\.

### Syntax<a name="realtime-script-objects-sendreliablegroupmessage-syntax"></a>

```
rtSession.newBinaryGameMessage(opcode, sender, binaryPayload)
```

## Player object<a name="realtime-script-objects-player"></a>

Access player\-related information\.

### player\.peerId<a name="realtime-script-objects-playerpeerid"></a>

Unique ID that is assigned to a game client when it connects to the Realtime server and joined the game session\.

### player\.playerSessionId<a name="realtime-script-objects-playersessionid"></a>

Player session ID that was referenced by the game client when it connected to the Realtime server and joined the game session\.

**Game message object**  
Use these methods to access messages that are received by the Realtime server\. Messages received from game clients have the [RTMessage](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-rtmessage) structure\.

## getPayloadAsText\(\)<a name="realtime-script-objects-getpayloadastext"></a>

Gets the game message payload as text\. 

### Syntax<a name="realtime-script-objects-getpayloadastext-syntax"></a>

```
gameMessage.getPayloadAsText()
```

## gameMessage\.opcode<a name="realtime-script-objects-gamemessageopcode"></a>

Operation code contained in a message\.

## gameMessage\.payload<a name="realtime-script-objects-gamemessagepayload"></a>

Payload contained in a message\. May be text or binary\.

## gameMessage\.sender<a name="realtime-script-objects-gamemessagesender"></a>

Peer ID of the game client that sent a message\.

## gameMessage\.reliable<a name="realtime-script-objects-gamemessagereliable"></a>

Boolean indicating whether the message was sent via TCP \(true\) or UDP \(false\)\.

## Configuration object<a name="realtime-script-objects-configuration"></a>

The configuration object can be used to override default configurations\.

### configuration\.maxPlayers<a name="realtime-script-objects-configuration-maxplayers"></a>

The maximum number of client / server connections that can be accepted by RealTimeServers\.

The default is 32\.

### configuration\.pingIntervalTime<a name="realtime-script-objects-configuration-pingIntervalTime"></a>

Time interval in milliseconds that server will attempt to send a ping to all connected clients to verify connections are healthy\.

The defailt is 3000ms\.