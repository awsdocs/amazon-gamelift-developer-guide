# Add GameLift to an O3DE game client and server<a name="game-client-intro"></a>

You can use O3DE, an open\-source, cross\-platform, real time 3D engine to create high performance interactive experiences, including games and simulations\. The O3DE renderer and tools are wrapped in a modular framework that you can modify and extend with your preferred development tools\.

The modular framework uses *Gems* that contain libraries with standard interfaces and assets\. Select your own Gems to choose what functionality to add based on your requirements\.

The GameLift Gem provides the following features:

**GameLift integration**  
A framework to extend the O3DE networking layer and to let the Multiplayer Gem work with the GameLift dedicated server solution\. The Gem provides integrations with both the [GameLift server SDK](reference-serversdk.md) and the AWS SDK client \(to call the GameLift service itself\)\.

**Build and package management**  
Instructions to package and optionally upload the dedicated server build and an AWS Cloud Development Kit \(AWS CDK\) \(AWS CDK\) application to set up and update resources\.

## GameLift Gem setup<a name="game-client-intro-setup"></a>

Follow the procedures in this section to set up the GameLift Gem in O3DE\.

**Prerequisites**
+ Set up your AWS account for GameLift\. For more information, see [Set up an AWS account](setting-up-aws-login.md)\.
+ Set up AWS credentials for O3DE\. For more information see, [Configuring AWS Credentials](https://www.o3de.org/docs/user-guide/gems/reference/aws/aws-core/configuring-credentials/)\.
+ Set up the AWS CLI and AWS CDK\. For more information, [AWS Command Line Interface](http://aws.amazon.com/cli/) and [AWS Cloud Development Kit \(AWS CDK\)](http://aws.amazon.com/cdk/)\.

**Turn on the GameLift Gem and its dependencies**

1. Open the **Project Manager**\.

1. Open the menu under your project and choose **Edit Project Setting\.\.\.**\.

1. Choose **Configure Gems**\.

1. Turn on the GameLift Gem and the following dependent Gems:
   + [AWS Core Gem](https://www.o3de.org/docs/user-guide/gems/reference/aws/aws-core/) – Provide the framework to use AWS services in O3DE\.
   + [Multiplayer Gem](https://www.o3de.org/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/) – Provides multiplayer functionality by extending the networking framework\.

**Include the GameLift Gem static library**

1. Include the `Gem::AWSGameLift.Server.Static` as `BUILD_DEPENDENCIES` for your project server target\.

   ```
   ly_add_target(
       NAME YourProject.Server.Static STATIC
       ...
       BUILD DEPENDCIES
           PUBLIC
               ...
           PRIVATE
               ...
               Gem::AWSGameLift.Server.Static
   )
   ```

1. Set `AWSGameLiftService` to required for your project server system component\.

   ```
   void YourProjectServerSystemComponent::GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required)
   {
       ...
       required.push_back(AZ_CRC_CE("AWSGameLiftServerService"));
       ...
   }
   ```

1. \(Optional\) To make GameLift service requests in C\+\+, include `Gem::AWSGameLift.Client.Static` in the `BUILD_DEPENDENCIES` for your client target\.

   ```
   ly_add_target(
       NAME YourProject.Client.Static STATIC
       ...
       BUILD_DEPENDENCIES
       PUBLIC
           ...
       PRIVATE
           ...
           Gem::AWSCore.Static
           Gem::AWSGameLift.Client.Static
   }
   ```

**Integrate your game and dedicated server**  
Manage game sessions within your game and dedicated game server with the [Session Management Integration](https://www.o3de.org/docs/user-guide/gems/reference/aws/aws-gamelift/session-management/integration/)\. To support FlexMatch, see [FlexMatch Integration](https://www.o3de.org/docs/user-guide/gems/reference/aws/aws-gamelift/flexmatch/integration/)\.