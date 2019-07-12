# Realtime Servers Client API \(C\#\) Reference: Asynchronous Callbacks<a name="realtime-sdk-csharp-ref-callbacks"></a>

Use this C\# Realtime Client API reference to help you prepare your multiplayer game for use with Realtime Servers deployed on Amazon GameLift fleets\. For details on the integration process, see [Get Started with Realtime Servers](realtime-plan.md)\.
+ [Synchronous Actions](realtime-sdk-csharp-ref-actions.md)
+ Asynchronous Callbacks
+ [Data Types](realtime-sdk-csharp-ref-datatypes.md)

A game client needs to implement these callback methods to respond to events\. The Realtime server invokes these callbacks to send game\-related information to the game client\. Callbacks for the same events can also be implemented with custom game logic in the Realtime server script\. See [Script Callbacks for Realtime Servers](realtime-script-callbacks.md)\.

Callback methods are defined in `ClientEvents.cs`\.

## OnOpen\(\)<a name="realtime-sdk-csharp-ref-callbacks-onopen"></a>

Invoked when the server process accepts the game client's connection request and opens a connection\.

### Syntax<a name="realtime-sdk-csharp-ref-callbacks-onopen-syntax"></a>

```
public void OnOpen()
```

### Parameters<a name="realtime-sdk-csharp-ref-callbacks-onopen-parameter"></a>

This method takes no parameters\.

### Return Value<a name="realtime-sdk-csharp-ref-callbacks-onopen-return"></a>

This method does not return anything\. 

## OnClose\(\)<a name="realtime-sdk-csharp-ref-callbacks-onclose"></a>

Invoked when the server process terminates the connection with the game client, such as after a game session ends\.

### Syntax<a name="realtime-sdk-csharp-ref-callbacks-onclose-syntax"></a>

```
public void OnClose()
```

### Parameters<a name="realtime-sdk-csharp-ref-callbacks-onclose-parameter"></a>

This method takes no parameters\.

### Return Value<a name="realtime-sdk-csharp-ref-callbacks-onclose-return"></a>

This method does not return anything\. 

## OnError\(\)<a name="realtime-sdk-csharp-ref-callbacks-onerror"></a>

Invoked when a failure occurs for a Realtime Client API request\. This callback can be customized to handle a variety of connection errors\.

### Syntax<a name="realtime-sdk-csharp-ref-callbacks-onerror-syntax"></a>

```
private void OnError(byte[] args)
```

### Parameters<a name="realtime-sdk-csharp-ref-callbacks-onerror-parameter"></a>

This method takes no parameters\.

### Return Value<a name="realtime-sdk-csharp-ref-callbacks-onerror-return"></a>

This method does not return anything\. 

## OnDataReceived\(\)<a name="realtime-sdk-csharp-ref-callbacks-ondata"></a>

Invoked when the game client receives a message from the Realtime server\. This is the primary method by which messages and notifications are received by a game client\.

### Syntax<a name="realtime-sdk-csharp-ref-callbacks-ondata-syntax"></a>

```
public void OnDataReceived(DataReceivedEventArgs dataReceivedEventArgs)
```

### Parameters<a name="realtime-sdk-csharp-ref-callbacks-ondata-parameter"></a>

**dataReceivedEventArgs**  
Information related to message activity\.  
Type: [DataReceivedEventArgs](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-dataeventargs)  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-callbacks-ondata-return"></a>

This method does not return anything\. 

## OnGroupMembershipUpdated\(\)<a name="realtime-sdk-csharp-ref-callbacks-ongroupupdate"></a>

Invoked when the membership for a group that the player belongs to has been updated\. This callback is also invoked when a client calls `RequestGroupMembership`\.

### Syntax<a name="realtime-sdk-csharp-ref-callbacks-ongroupupdate-syntax"></a>

```
public void OnGroupMembershipUpdated(GroupMembershipEventArgs groupMembershipEventArgs)
```

### Parameters<a name="realtime-sdk-csharp-ref-callbacks-ongroupupdate-parameter"></a>

**groupMembershipEventArgs**  
Information related to group membership activity\.   
Type: [GroupMembershipEventArgs](realtime-sdk-csharp-ref-datatypes.md#realtime-sdk-csharp-ref-datatypes-groupeventargs)  
Required: Yes

### Return Value<a name="realtime-sdk-csharp-ref-callbacks-ongroupupdate-return"></a>

This method does not return anything\. 