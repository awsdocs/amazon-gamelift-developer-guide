# Design your Backend Service<a name="gamelift_quickstart_customservers_designbackend"></a>

You should always implement a backend service that authenticates your players and communicates with the Amazon GameLift APIs\. Implementing a custom backend service has the following benefits: 
+ You can customize authentication for your players\.
+ You control how matching and game session placement are initiated\.
+ The backend service is authoritative\. You can use your player database for player attributes such as skill rating for matchmaking instead of trusting the client\. 

![\[Hosting component flow\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_game_backend_options.png)

 You can build a serverless backend using services such as Amazon API Gateway, AWS Lambda, and Amazon DynamoDB to minimize your operational efforts, scale seamlessly to your needs, and pay for exactly what you use\. In case you need more control, you can build container\-based backends using an Application Load Balancer and Amazon Elastic Container Service or Amazon Elastic Kubernetes Service\. In addition to containers, you can always host your backend application directly on virtual machines with EC2 instances behind an Application Load Balancer and leverage Auto Scaling groups to scale the capacity\. You can use AWS AppSync to build a GraphQL based backend for your game using WebSockets\. 

For an example implementation with a serverless backend, see [ GameLift Example with Serverless Backend](https://github.com/aws-samples/aws-gamelift-and-serverless-backend-sample)\. To learn more about building an API with Lambda integrations, see [ Build an API Gateway REST API with Lambda integration](https://docs.aws.amazon.com/https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-lambda-integration.html)\. For step\-by\-step instructions on using DynamoDB to model player data, see [ Introduction: Modeling Game Player Data with Amazon DynamoDB](https://aws.amazon.com/getting-started/hands-on/data-modeling-gaming-app-with-dynamodb/)\. 

## Authenticating your Players<a name="gamelift_quickstart_customservers_designbackend_auth"></a>

You can use Amazon Cognito to authenticate your game clients\. Amazon Cognito can be used to manage the whole lifecycle and properties of your player identities with Amazon Cognito user pools, or you can integrate to your custom identities and external identities like Google, Apple ID, or Facebook with the more lightweight Amazon Cognito identity pools option that is used in the examples\. 

You can also build a custom identity solution or use a third\-party identity solution that you host on AWS or is managed for you\. You can use Lambda authorizers for custom authorization logic with Amazon API Gateway\. 

GameLift hosting provides player session IDs\. You can use a player session ID to make sure that the correct player has joined the correct game server\. For GameLift FleetIQ, you must implement this type of validation yourself\. 

**Additional resources:**
+ [ Using Cognito Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html) 
+ [Using Getting Started with Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-cognito-user-pools.html)
+ [How to Set Up Player Authentication for Game Servers with Amazon Cognito](https://aws.amazon.com/blogs/gametech/how-to-set-up-player-authentication-with-amazon-cognito/)