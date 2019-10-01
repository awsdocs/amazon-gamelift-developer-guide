# Realtime Servers Client API \(C\#\) Reference: Actions<a name="realtime-sdk-csharp-ref-actions"></a>

This C\# Realtime Client API reference can help you prepare your multiplayer game for use with Realtime Servers deployed on Amazon GameLift fleets\. For details on the integration process, see [Get Started with Realtime Servers](realtime-plan.md)\.
+ Synchronous Actions
+ [Asynchronous Callbacks](realtime-sdk-csharp-ref-callbacks.md)
+ [Data Types](realtime-sdk-csharp-ref-datatypes.md)

## Client\(\)<a name="realtime-sdk-csharp-ref-actions-client"></a>

Initializes a new client to communicate with the Realtime server and identifies the type of connection to use\. 

### Syntax<a name="integration-server-sdk-csharp-ref-actions-client-syntax"></a>

```
public Client(ClientConfiguration configuration)
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-client-parameter"></a>

**clientConfiguration**  
Configuration details specifying the client/server connection type\. You can opt to call Client\(\) without this parameter; however, this approach results in an unsecured connection by default\.  
Type: [ClientConfiguration](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-clientconfiguration)  
Required: No

### Return Value<a name="realtime-sdk-csharp-ref-actions-client-return"></a>

Returns an instance of the Realtime client for use with communicating with the Realtime server\. 

## Connect\(\)<a name="realtime-sdk-csharp-ref-actions-connect"></a>

Requests a connection to a server process that is hosting a game session\. 

### Syntax<a name="integration-server-sdk-csharp-ref-actions-connect-syntax"></a>

```
public ConnectionStatus Connect(string endpoint, int remoteTcpPort, int listenPort, ConnectionToken token)
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-connect-parameter"></a>

**endpoint**  
DNS name or IP address of the game session to connect to\. The endpoint is specified in a `GameSession` object, which is returned in response to a client call to the *AWS SDK Amazon GameLift API* actions [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html), [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html), or [DescribeGameSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_SearchGameSessions.html)\.   
If the Realtime server is running on a fleet with a TLS certificate, you must use the DNS name\. 
Type: String  
Required: Yes

**remoteTcpPort**  
Port number for the TCP connection assigned to the game session\. This information is specified in a `GameSession` object, which is returned in response to a [StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html) [CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html), or [DescribeGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSession.html) request\.  
Type: Integer  
Valid Values: 1900 to 2000\.  
Required: Yes

**listenPort**  
Port number that the game client is listening on for messages sent using the UDP channel\.   
Type: Integer  
Valid Values: 33400 to 33500\.  
Required: Yes

**token**  
Optional information that identifies the requesting game client to the server process\.   
Type: [ConnectionToken](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-connectiontoken)  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-actions-connect-return"></a>

Returns a `ConnectionStatus` [enum]() value indicating the client's connection status\. 

## Disconnect\(\)<a name="realtime-sdk-csharp-ref-actions-disconnect"></a>

When connected to a game session, disconnects the game client from the game session\. 

### Syntax<a name="realtime-sdk-csharp-ref-actions-disconnect-syntax"></a>

```
public void Disconnect()
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-disconnect-parameter"></a>

This action has no parameters\.

### Return Value<a name="realtime-sdk-csharp-ref-actions-disconnect-return"></a>

This method does not return anything\.

## NewMessage\(\)<a name="realtime-sdk-csharp-ref-actions-newmessage"></a>

Creates a new message object with a specified operation code\. Once a message object is returned, complete the message content by specifying a target, updating the delivery method, and adding a data payload as needed\. Once completed, send the message using `SendMessage()`\.

### Syntax<a name="realtime-sdk-csharp-ref-actions-newmessage-syntax"></a>

```
public RTMessage NewMessage(int opCode)
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-newmessage-parameter"></a>

**opCode**  
Developer\-defined operation code that identifies a game event or action, such as a player move or a server notification\.   
Type: Integer  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-actions-newmessage-return"></a>

Returns an [RTMessage](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-rtmessage) object containing the specified operation code and default delivery method\. The delivery intent parameter is set to `FAST` by default\. 

## SendMessage\(\)<a name="realtime-sdk-csharp-ref-actions-sendmessage"></a>

 Sends a message to a player or group using the delivery method specified\.

### Syntax<a name="realtime-sdk-csharp-ref-actions-sendmessage-syntax"></a>

```
public void SendMessage(RTMessage message)
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-sendmessage-parameter"></a>

**message**  
Message object that specifies the target recipient, delivery method, and message content\.   
Type: [RTMessage](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-rtmessage)  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-actions-sendmessage-return"></a>

This method does not return anything\. 

## JoinGroup\(\)<a name="realtime-sdk-csharp-ref-actions-joingroup"></a>

Adds the player to the membership of a specified group\. Groups can contain any of the players that are connected to the game\. Once joined, the player receives all future messages sent to the group and can send messages to the entire group\.

### Syntax<a name="realtime-sdk-csharp-ref-actions-joingroup-syntax"></a>

```
public void JoinGroup(int targetGroup)
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-joingroup-parameter"></a>

**targetGroup**  
Unique ID that identifies the group to add the player to\. Group IDs are developer\-defined\.  
Type: Integer  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-actions-joingroup-return"></a>

This method does not return anything\. Because this request is sent using the reliable \(TCP\) delivery method, a failed request triggers the callback [OnError\(\)](realtime-sdk-csharp-ref-callbacks.md#realtime-sdk-csharp-ref-callbacks-onerror)\. 

## LeaveGroup\(\)<a name="realtime-sdk-csharp-ref-actions-leavegroup"></a>

Removes the player from the membership of a specified group\. Once no longer in the group, the player does not receive messages sent to the group and cannot send messages to the entire group\.

### Syntax<a name="integration-server-sdk-csharp-ref-actions-leavegroup-syntax"></a>

```
public void LeaveGroup(int targetGroup)
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-leavegroup-parameter"></a>

**targetGroup**  
Unique ID identifying the group to remove the player from\. Group IDs are developer\-defined\.  
Type: Integer  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-actions-leavegroup-return"></a>

This method does not return anything\. Because this request is sent using the reliable \(TCP\) delivery method, a failed request triggers the callback [OnError\(\)](realtime-sdk-csharp-ref-callbacks.md#realtime-sdk-csharp-ref-callbacks-onerror)\.

## RequestGroupMembership\(\)<a name="realtime-sdk-csharp-ref-actions-requestgroupmembership"></a>

Requests that a list of players in the specified group be sent to the game client\. Any player can request this information, regardless of whether they are a member of the group or not\. In response to this request, the membership list is sent to the client via an [OnGroupMembershipUpdated\(\)](realtime-sdk-csharp-ref-callbacks.md#realtime-sdk-csharp-ref-callbacks-ongroupupdate) callback\.

### Syntax<a name="realtime-sdk-csharp-ref-actions-requestgroupmembership-syntax"></a>

```
public void RequestGroupMembership(int targetGroup)
```

### Parameters<a name="realtime-sdk-csharp-ref-actions-requestgroupmembership-parameter"></a>

**targetGroup**  
Unique ID identifying the group to get membership information for\. Group IDs are developer\-defined\.  
Type: Integer  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-actions-requestgroupmembership-return"></a>

This method does not return anything\. 