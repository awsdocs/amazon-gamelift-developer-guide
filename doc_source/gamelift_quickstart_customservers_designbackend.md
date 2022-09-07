# Design your backend service<a name="gamelift_quickstart_customservers_designbackend"></a>

Always implement a backend service that authenticates your players and communicates with the GameLift API operations\. A custom backend service has the following benefits:
+ You can customize authentication for your players\.
+ You control how to initiate matching and game session placement\.
+ The backend service is authoritative\. You can use player attributes from your player database \(for example, skill rating\) for matchmaking instead of trusting the client\.

You can build a serverless backend using AWS services such as Amazon API Gateway, AWS Lambda, and Amazon DynamoDB\. Use these services to help minimize your operational efforts, scale seamlessly to your needs, and pay for exactly what you use\. If you need more control, you can build container\-based backends\. Build these using an Application Load Balancer and Amazon Elastic Container Service \(Amazon ECS\) or Amazon Elastic Kubernetes Service \(Amazon EKS\)\. You can also host your backend application directly on virtual machines with Amazon Elastic Compute Cloud \(Amazon EC2\) instances behind an Application Load Balancer\. With this configuration, you can leverage Amazon EC2 Auto Scaling groups to scale the capacity\. In addition to these backend options, you can use AWS AppSync to build a GraphQL\-based backend for your game using WebSockets\.

![\[AWS services that you can use to design your backend service. Build a container-based backend using Elastic Load Balancing with Amazon ECS or Amazon EKS. Or, use Elastic Load Balancing with Amazon EC2. Build a serverless backend using API Gateway with Lambda. Build a GraphQL-based backend using AWS AppSync.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_game_backend_options.png)

For an example implementation with a serverless backend, see [Multiplayer Session\-based Game Hosting on AWS](https://github.com/aws-samples/aws-gamelift-and-serverless-backend-sample) on GitHub\. For more information about using API Gateway with Lambda, see [Build an API Gateway REST API with Lambda integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-lambda-integration.html) in the *API Gateway Developer Guide*\. For instructions on using DynamoDB to model player data, see [Introduction: Modeling Game Player Data with Amazon DynamoDB](http://aws.amazon.com/getting-started/hands-on/data-modeling-gaming-app-with-dynamodb/)\.

## Authenticating your players<a name="gamelift_quickstart_customservers_designbackend_auth"></a>

To authenticate your game clients, you can use Amazon Cognito\. With Amazon Cognito user pools, you can manage the whole lifecycle and properties of your player identities\. Or, you can use Amazon Cognito identity pools with external identity providers \(IdPs\) from Google, Apple, or Facebook\. 

You can also build a custom identity solution or use a third\-party IdP solution that you host on AWS, or that AWS manages for you\. Additionally, you can use [Lambda authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html) for custom authorization logic with API Gateway\. 

GameLift hosting provides player session IDs\. You can use these IDs to make sure that players join the correct game servers\. For GameLift FleetIQ, you must implement this type of validation yourself\.

**Related information**
+ [Using identity pools \(federated identities\)](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html)
+ [Getting started with user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-cognito-user-pools.html)
+ [How to Set Up Player Authentication with Amazon Cognito](http://aws.amazon.com/blogs/gametech/how-to-set-up-player-authentication-with-amazon-cognito/)