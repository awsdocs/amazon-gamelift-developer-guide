# Realtime Servers Client API \(C\#\) Reference: Data Types<a name="realtime-sdk-csharp-ref-datatypes"></a>

This C\# Realtime Client API reference can help you prepare your multiplayer game for use with Realtime Servers deployed on Amazon GameLift fleets\. For details on the integration process, see [Get Started with Realtime Servers](realtime-plan.md)\.
+ [Synchronous Actions](realtime-sdk-csharp-ref-actions.md)
+ [Asynchronous Callbacks](realtime-sdk-csharp-ref-callbacks.md)
+ Data Types

## ClientConfiguration<a name="realtime-sdk-csharp-ref-datatypes-clientconfiguration"></a>

Information about how the game client connects to a Realtime server\. 

### Contents<a name="realtime-sdk-csharp-ref-datatypes-clientconfiguration-contents"></a>

**ConnectionType**  
Type of client/server connection to use, either secured or unsecured\. If you don't specify a connection type, the default is unsecured\.   
When connecting to a Realtime server on a secured fleet with a TLS certificate, you must use the value RT\_OVER\_WSS\_DTLS\_TLS12\. 
Type: A `ConnectionType` [enum]() value\.  
Required: No

## ConnectionToken<a name="realtime-sdk-csharp-ref-datatypes-connectiontoken"></a>

Information about the game client and/or player that is requesting a connection with a Realtime server\.

### Contents<a name="realtime-sdk-csharp-ref-datatypes-connectiontoken-contents"></a>

**playerSessionId**  
Unique ID issued by GameLift when a new player session is created\. A player session ID is specified in a `PlayerSession` object, which is returned in response to a client call to the *AWS SDK Amazon GameLift API* actions [ StartGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartGameSessionPlacement.html), [ CreateGameSession](https://docs.aws.amazon.com/gamelift/latest/apireference/API_CreateGameSession.html), [ DescribeGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeGameSessionPlacement.html), or [ DescribePlayerSessions](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribePlayerSessions.html)\.  
Type: String  
Required: No

**payload**  
Developer\-defined information to be communicated to the Realtime server on connection\. This includes any arbitrary data that might be used for a custom sign\-in mechanism\. For examples, a payload may provide authentication information to be processed by the Realtime server script before allowing a client to connect\.  
Type: byte array   
Required: No

## RTMessage<a name="realtime-sdk-csharp-ref-datatypes-rtmessage"></a>

Content and delivery information for a message\. A message must specify either a target player or a target group\. 

### Contents<a name="realtime-sdk-csharp-ref-datatypes-rtmessage-contents"></a>

**opCode**  
Developer\-defined operation code that identifies a game event or action, such as a player move or a server notification\. A message's Op code provides context for the data payload that is being provided\. Messages that are created using NewMessage\(\) already have the operation code set, but it can be changed at any time\.   
Type: Integer  
Required: Yes

**targetPlayer**  
Unique ID identifying the player who is the intended recipient of the message being sent\. The target may be the server itself \(using the server ID\) or another player \(using a player ID\)\.   
Type: Integer   
Required: No

**targetGroup**  
Unique ID identifying the group that is the intended recipient of the message being sent\. Group IDs are developer defined\.   
Type: Integer   
Required: No

**deliveryIntent**  
Indicates whether to send the message using the reliable TCP connection or using the fast UDP channel\. Messages created using [NewMessage\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-newmessage)\.  
Type: DeliveryIntent enum  
Valid values: FAST \| RELIABLE  
Required: Yes

**payload**  
Message content\. This information is structured as needed to be processed by the game client based on the accompanying operation code\. It may contain game state data or other information that needs to be communicated between game clients or between a game client and the Realtime server\.  
Type: Byte array   
Required: No

## DataReceivedEventArgs<a name="realtime-sdk-csharp-ref-datatypes-dataeventargs"></a>

Data provided with an [OnDataReceived\(\)](realtime-sdk-csharp-ref-callbacks.md#realtime-sdk-csharp-ref-callbacks-ondata) callback\. 

### Contents<a name="realtime-sdk-csharp-ref-datatypes-dataeventargs-contents"></a>

**sender**  
Unique ID identifying the entity \(player ID or server ID\) who originated the message\.  
Type: Integer   
Required: Yes

**opCode**  
Developer\-defined operation code that identifies a game event or action, such as a player move or a server notification\. A message's Op code provides context for the data payload that is being provided\.   
Type: Integer   
Required: Yes

**data**  
Message content\. This information is structured as needed to be processed by the game client based on the accompanying operation code\. It may contain game state data or other information that needs to be communicated between game clients or between a game client and the Realtime server\.  
Type: Byte array   
Required: No

## GroupMembershipEventArgs<a name="realtime-sdk-csharp-ref-datatypes-groupeventargs"></a>

Data provided with an [OnGroupMembershipUpdated\(\)](realtime-sdk-csharp-ref-callbacks.md#realtime-sdk-csharp-ref-callbacks-ongroupupdate) callback\. 

### Contents<a name="realtime-sdk-csharp-ref-datatypes-groupeventargs-contents"></a>

**sender**  
Unique ID identifying the player who requested a group membership update\.  
Type: Integer   
Required: Yes

**opCode**  
Developer\-defined operation code that identifies a game event or action\.   
Type: Integer   
Required: Yes

**groupId**  
Unique ID identifying the group that is the intended recipient of the message being sent\. Group IDs are developer defined\.  
Type: Integer   
Required: Yes

**playerId**  
List of player IDs who are current members of the specified group\.   
Type: Integer array   
Required: Yes

## Enums<a name="realtime-sdk-csharp-ref-datatypes-enums"></a>

Enums defined for the Realtime Client SDK are defined as follows: 

**ConnectionStatus**  
+ CONNECTED – Game client is connected to the Realtime server with a TCP connection only\. All messages regardless of delivery intent are sent via TCP\.
+ CONNECTED\_SEND\_FAST – Game client is connected to the Realtime server with a TCP and a UDP connection\. However, the ability to receive messages via UDP is not yet verified; as a result, all messages sent to the game client use TCP\. 
+ CONNECTED\_SEND\_AND\_RECEIVE\_FAST – Game client is connected to the Realtime server with a TCP and a UDP connection\. The game client can send and receive messages using either TCP or UDP\. 
+ CONNECTING Game client has sent a connection request and the Realtime server is processing it\.
+ DISCONNECTED\_CLIENT\_CALL – Game client was disconnected from the Realtime server in response to a [Disconnect\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-disconnect)request from the game client\.
+ DISCONNECTED – Game client was disconnected from the Realtime server for a reason other than a client disconnect call\.

**ConnectionType**  
+ RT\_OVER\_WSS\_DTLS\_TLS12 – Secure connection type\. 

  For use with Realtime servers that are running on a GameLift fleet with a TLS certificate generated\. When using a secure connection, TCP traffic is encrypted using TLS 1\.2, and UDP traffic is encrypted using DTLS 1\.2\.
+ RT\_OVER\_WS\_UDP\_UNSECURED – Non\-secure connection type\.
+ RT\_OVER\_WEBSOCKET – Non\-secure connection type\. This value is no longer preferred\.

**DeliveryIntent**  
+ FAST – Delivered using a UDP channel\. 
+ RELIABLE – Delivered using a TCP connection\. 