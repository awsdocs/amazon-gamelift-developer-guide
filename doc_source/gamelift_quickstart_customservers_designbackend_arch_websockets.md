# Standalone game session servers with WebSockets\-based backend<a name="gamelift_quickstart_customservers_designbackend_arch_websockets"></a>

The serveless architecture can be modified to use API Gateway WebSockets with GameLift\. Using this architecture, you can make matchmaking requests with WebSockets, and push notifications of matchmaking completion \(or failure\) using server\-initiated messages over WebSockets\. This architecture improves performance by having a two\-way communication between the client and the server\. The matchmaking result is received immediately after success\. WebSockets increases complexity because the WebSocket connections from clients need to be managed with a set of AWS Lambda functions \(`OnConnect` and `OnDisconnect`\)\. A database like Amazon DynamoDB is needed to store the connections\. 

![\[Example WebSockets architecture\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_arch_websockets.png)

The diagram shows the process of getting players into games running on the GameLift game servers using WebSockets\. It includes the following steps:

1. Game client requests an Amazon Cognito identity from an Amazon Cognito identity pool\. This can be connected optionally to external identity providers such as Google, Apple, Xbox Live, and Steam identities\.

1. Game client signs a WebSocket connection to the API Gateway with the Amazon Cognito credentials\.

1. API Gateway calls the `OnConnect` Lambda function on the connection\. The connection information is stored in an Amazon DynamoDB table\.

1. Game client sends a message over the WebSocket connection to request a session\.

1. A Lambda function receives the message and requests a match through FlexMatch matchmaking\. FlexMatch allows you to define the matchmaking configuration with JSON\-based configuration documents\. The game client can send latency data against the different Regions in addition to allowing latency\-based matchmaking\.

1. After FlexMatch has matched a suitable group of players with suitable latency to a Region, it will request a game session placement through a GameLift queue that has fleets with one or more Region locations registered to it\.

1. When the session is placed on one of the fleets, an event notification is sent to an Amazon SNS topic\.

1. A Lambda function will receive the Amazon SNS event and process it\.

1. If the event is a `MatchmakingSucceeded` event, the Lambda function requests the correct player connection from DynamoDB and uses the API Gateway APIs to send a message directly to that player over the WebSocket\. The game client does not need to actively poll the status of matchmaking in this model\.

1. Game client receives the port and IP address of the game server as well as the player session ID through the WebSocket connection\.

1. The game client connects to the game server using TCP or UDP using the port and IP address provided by the backend\. It also sends the player session ID to the game server and the game server can validate it using the GameLift Server SDK\.