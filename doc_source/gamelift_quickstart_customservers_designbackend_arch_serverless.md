# Standalone game session servers with a serverless backend<a name="gamelift_quickstart_customservers_designbackend_arch_serverless"></a>

You can use a serverless backend with your standalone game session servers in GameLift\.

The following architecture shows how to use a serverless implementation for a backend service where the game client authenticates against Amazon API Gateway with an Amazon Cognito identity to access backend services running in Lambda functions\. The backend Lambda functions communicate with GameLift APIs to request game sessions through FlexMatch\. Matchmaking events are received through an Amazon SNS event and stored in an Amazon DynamoDB NoSQL database by a Lambda function\. Using this architecture, the backend can check the status of the matchmaking tickets from a highly scalable database instead of directly accessing the GameLift APIs\. 

![\[Example serverless architecture\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_arch_serverless.png)

The diagram shows the process of getting players into games running on the GameLift game servers\. It includes the following steps:

1. Game client requests an Amazon Cognito identity from an Amazon Cognito identity pool\. This can be connected optionally to external identity providers such as Google, Apple, Xbox Live, and Steam identities\.

1. The game client receives temporary access credentials and requests a game session through an API hosted with API Gateway by signing the request with these credentials\.

1. API Gateway invokes a Lambda function\.

1. The Lambda function requests player data from a DynamoDB NoSQL table\. The Amazon Cognito identity can be used to securely request the correct player data because the authenticated identity is provided in the request context data\.

1. With the correct player data for any additional information like player skill level, the Lambda function requests a match through FlexMatch matchmaking\. You can define a FlexMatch matchmaking configuration with JSON\-based configuration documents\. The game client can send latency data against the different Regions and can allow latency\-based matchmaking\.

1. After FlexMatch matches a suitable group of players with suitable latency to a Region, it requests a game session placement through a GameLift queue\. The queue has fleets with one or more Region locations registered to it\.

1. When the session is placed on one of the fleet's locations, an event notification is sent to an Amazon SNS topic\.

1. A Lambda function will receive the Amazon SNS event and process it\.

1. If the ticket is a `MatchmakingSucceeded` event, the Lambda function writes the result to DynamoDB with the server port and IP address\. A time\-to\-live \(TTL\) value is used to make sure that the tickets are deleted from DynamoDB when they are not needed anymore\.

1. The game client makes a signed request to API Gateway to check the status of the matchmaking ticket on a specific interval\.

1. API Gateway invokes a Lambda function that checks the matchmaking ticket status\. 

1. The Lambda function checks DynamoDB to see if the ticket has already succeeded\. If it has succeeded, the Lambda function sends the IP address, port, and the player session ID back to the client\. If not, it sends a response that the match is not ready yet\.

1. The game client connects to the game server using TCP or UDP by using the port and IP address provided by the backend\. It sends the player session ID to the game server and the game server can validate it using the GameLift Server SDK\. 