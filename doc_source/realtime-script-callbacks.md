# Script Callbacks for Realtime Servers<a name="realtime-script-callbacks"></a>

You can provide custom logic to respond to events by implementing these callbacks in your Realtime script\.

## init<a name="realtime-script-callback-init"></a>

Initializes the Realtime server and receives a Realtime server interface\. 

### Syntax<a name="realtime-script-callback-init-syntax"></a>

```
init(rtsession)
```

## onMessage<a name="realtime-script-callback-onmessage"></a>

Invoked when a received message is sent to the server\. 

### Syntax<a name="realtime-script-callback-onmessage-syntax"></a>

```
onMessage(gameMessage)
```

## onHealthCheck<a name="realtime-script-callback-onhealthcheck"></a>

Invoked to set the status of the game session health\. By default, health status is healthy \(or `true`\. This callback can be implemented to perform custom health checks and return a status\.

### Syntax<a name="realtime-script-callback-onhealthcheck-syntax"></a>

```
onHealthCheck()
```

## onStartGameSession<a name="realtime-script-callback-onstartgamesession"></a>

Invoked when a new game session starts, with a game session object passed in\.

### Syntax<a name="realtime-script-callback-onstartgamesession-syntax"></a>

```
onStartGameSession(session)
```

## onProcessTerminate<a name="realtime-script-callback-onprocessterminate"></a>

Invoked when the server process is being terminated by the Amazon GameLift service\. This can act as a trigger to exit cleanly from the game session\. There is no need to call `processEnding().`

### Syntax<a name="realtime-script-callback-onprocessterminate-syntax"></a>

```
onProcessTerminate()
```

## onPlayerConnect<a name="realtime-script-callback-onplayerconnect"></a>

Invoked when a player requests a connection and has passed initial validation\. 

### Syntax<a name="realtime-script-callback-onplayerconnect-syntax"></a>

```
onPlayerConnect(connectMessage)
```

## onPlayerAccepted<a name="realtime-script-callback-onplayeraccepted"></a>

Invoked when a player connection is accepted\.

### Syntax<a name="realtime-script-callback-onplayeraccepted-syntax"></a>

```
onPlayerAccepted(player)
```

## onPlayerDisconnect<a name="realtime-script-callback-onplayerdisconnect"></a>

Invoked when a player disconnects from the game session, either by sending a disconnect request or by other means\.

### Syntax<a name="realtime-script-callback-onplayerdisconnect-syntax"></a>

```
onPlayerDisconnect(peerId)
```

## onProcessStarted<a name="realtime-script-callback-onprocessstarted"></a>

Invoked when a server process is started\. This callback allows the script to perform any custom tasks needed to prepare to host a game session\. 

### Syntax<a name="realtime-script-callback-onprocessstarted-syntax"></a>

```
onProcessStarted(args)
```

## onSendToPlayer<a name="realtime-script-callback-onsendtoplayer"></a>

Invoked when a message is received on the server from one player to be delivered to another player\. This process runs before the message is delivered\. 

### Syntax<a name="realtime-script-callback-onsendtoplayer-syntax"></a>

```
nSendToPlayer(gameMessage)
```

## onSendToGroup<a name="realtime-script-callback-onsendtogroup"></a>

Invoked when a message is received on the server from one player to be delivered to a group\. This process runs before the message is delivered\. 

### Syntax<a name="realtime-script-callback-onsendtogroup-syntax"></a>

```
onSendToGroup(gameMessage))
```

## onPlayerJoinGroup<a name="realtime-script-callback-onplayerjoingroup"></a>

Invoked when a player sends a request to join a group\.

### Syntax<a name="realtime-script-callback-onplayerjoingroup-syntax"></a>

```
onPlayerJoinGroup(groupId, peerId)
```

## onPlayerLeaveGroup<a name="realtime-script-callback-onplayerleavegroup"></a>

Invoked when a player sends a request to leave a group\.

### Syntax<a name="realtime-script-callback-onplayerleavegroup-syntax"></a>

```
onPlayerLeaveGroup(groupId, peerId)
```