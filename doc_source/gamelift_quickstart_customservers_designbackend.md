# Design your backend service<a name="gamelift_quickstart_customservers_designbackend"></a>

We recommend that you implement a backend service that authenticates your players and communicates with the Amazon GameLift API\. By implementing a custom backend service, you can:
+ Customize authentication for your players\.
+ Control how GameLift matches and starts game sessions\.
+ Use your player database for player attributes such as skill rating for matchmaking instead of trusting the client\.

The following diagram shows some possible combinations of AWS services that you can use to create a backend service\. For example, Elastic Load Balancing plus Amazon Elastic Compute Cloud \(Amazon EC2\), Amazon Elastic Container Service \(Amazon ECS\), or Amazon Elastic Kubernetes Service \(Amazon EKS\)\. Or, Amazon API Gateway plus AWS Lambda\. Or, AWS AppSync plus Lambda\.

![\[Use different combinations of AWS services to create a backend service.\]](http://docs.aws.amazon.com/gamelift/latest/developerguide/images/qs_game_backend_options.png)

You can also build a serverless backend using API Gateway, Lambda, and Amazon DynamoDB\. If you need more control, then you can build container\-based backends using an Application Load Balancer and Amazon ECS or Amazon EKS\. You can also host your backend application directly with EC2 instances behind an Application Load Balancer\. Then, you can use Amazon EC2 Auto Scaling groups to scale the capacity\. You can also use AWS AppSync to build a GraphQL\-based backend for your game using WebSockets\.

For an example implementation with a serverless backend, see [Multiplayer Session\-based Game Hosting on AWS](https://github.com/aws-samples/aws-gamelift-and-serverless-backend-sample#multiplayer-session-based-game-hosting-on-aws) on GitHub\. For instructions about using DynamoDB to model player data, see [Introduction: Modeling Game Player Data with Amazon DynamoDB](http://aws.amazon.com/getting-started/hands-on/data-modeling-gaming-app-with-dynamodb/)\.

## Authenticating your players<a name="gamelift_quickstart_customservers_designbackend_auth"></a>

You can use Amazon Cognito to authenticate your game clients\. To manage the lifecycle and properties of your player identities, use Amazon Cognito user pools\. To integrate your custom identities and external identities, use Amazon Cognito identity pools\.

If you prefer, build a custom identity solution and host it on AWS\. You can also use Lambda authorizers for custom authorization logic with API Gateway\.

GameLift hosting provides player session IDs, which you can use to make sure that players join the correct game server\.

**Additional resources:**
+ [Using identity pools \(federated identities\)](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html) \(Amazon Cognito Developer Guide\)
+ [Getting started with user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-cognito-user-pools.html) \(Amazon Cognito Developer Guide\)
+ [How to Set Up Player Authentication with Amazon Cognito](http://aws.amazon.com/blogs/gametech/how-to-set-up-player-authentication-with-amazon-cognito/) \(AWS for Games Blog\)