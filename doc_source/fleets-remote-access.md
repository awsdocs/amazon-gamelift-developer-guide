# Remotely Access Fleet Instances<a name="fleets-remote-access"></a>

You can remotely access any fleet instance that is currently running in an Amazon GameLift fleet\. This capability is useful for troubleshooting fleet activation issues\. You can also use this feature to get real\-time game server activity, such as track log updates or run benchmarking tools using actual player traffic\. 

When remotely accessing individual Amazon GameLift instances, keep the following in mind:
+ The Amazon GameLift service continues to manage fleet activity and capacity\. Establishing a remote connection to an instance does not affect how Amazon GameLift manages it in any way\. As a result, the instance continues to execute the fleet runtime configuration, stop and start server processes, create and terminate game sessions, and allow player connections\. In addition, the Amazon GameLift service may terminate the instance at any time as part of a scale down event\.
+ Making local changes to an instance that is hosting active game sessions and has live players connected may significantly affect player experience\. For example, your local changes have the potential to drop individual players, crash game sessions or even shut down the entire instance with multiple game sessions and players affected\.

For more information on how games are deployed and managed on Amazon GameLift instances, see the following topics:
+ [How Amazon GameLift Works](gamelift-howitworks.md)
+ [Debug Fleet Issues](fleets-creating-debug.md)
+ [How a Fleet Manages Multiple Processes](fleets-multiprocess.md#fleets-multiprocess-howitworks)

## Connect to an Instance<a name="fleets-remote-access-connect"></a>

You can access remote instances that are running either Windows or Linux\. To connect to a Windows instance, use a remote desktop protocol \(RDP\) client\. To connect to a Linux instance, use an SSH client\. 

 Use the AWS CLI get the information you need to access a remote instance\. For help, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/gamelift/)\. [Get and install the AWS Command Line Interface tool\.](https://aws.amazon.com/cli/) You can also use the AWS SDK, with documentation available in the [Amazon GameLift Service API Reference](https://docs.aws.amazon.com/gamelift/latest/apireference/)\.

1. **Find the ID of the instance you want to connect to\.** When requesting access, you must specify an instance ID\. Use the AWS CLI command [describe\-instances](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-instances.html) \(or the API call [DescribeInstances](https://docs.aws.amazon.com/gamelift/latest/apireference/API_DescribeInstances.html)\) with a fleet ID to get information on all instances in the fleet\. For help, including example requests and responses, see the CLI or API reference guides\. 

1. **Request access credentials for the instance\.** Once you have an instance ID, use the command [get\-instance\-access](https://docs.aws.amazon.com/cli/latest/reference/gamelift/get-instance-access.html) \(or the API call [GetInstanceAccess](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GetInstanceAccess.html)\) to request access credentials and other information\. For help, including example requests and responses, see the CLI or API reference guides\. If successful, Amazon GameLift returns the instance's operating system, IP address, and a set of credentials \(user name and secret key\)\. The credentials format depends on the instance operating system\. Use the following instructions to retrieve credentials for either RDP or SSH\. 
   + **For Windows instances** – To connect to a Windows instance, RDP requires a user name and password\. The `get-instance-access` request returns these values as simple strings, so you can use the returned values as is\. Example credentials: 

     ```
     "Credentials": {
         "Secret": "aA1bBB2cCCd3EEE", 
         "UserName": "gl-user-remote"
     }
     ```
   + **For Linux instances** – To connect to a Linux instance, SSH requires a user name and private key\. Amazon GameLift issues RSA private keys and returns them as a single string, with the newline character \(`\n`\) indicating line breaks\. To make the private key usable, you must \(1\) convert the string to a `.pem` file, and \(2\) set permissions for the new file\. Example credentials returned: 

     ```
     "Credentials": {
         "Secret": "-----BEGIN RSA PRIVATE KEY-----nEXAMPLEKEYKCAQEAy7WZhaDsrA1W3mRlQtvhwyORRX8gnxgDAfRt/gx42kWXsT4rXE/b5CpSgie/\nvBoU7jLxx92pNHoFnByP+Dc21eyyz6CvjTmWA0JwfWiW5/akH7iO5dSrvC7dQkW2duV5QuUdE0QW\nZ/aNxMniGQE6XAgfwlnXVBwrerrQo+ZWQeqiUwwMkuEbLeJFLhMCvYURpUMSC1oehm449ilx9X1F\nG50TCFeOzfl8dqqCP6GzbPaIjiU19xX/azOR9V+tpUOzEL+wmXnZt3/nHPQ5xvD2OJH67km6SuPW\noPzev/D8V+x4+bHthfSjR9Y7DvQFjfBVwHXigBdtZcU2/wei8D/HYwIDAQABAoIBAGZ1kaEvnrqu\n/uler7vgIn5m7lN5LKw4hJLAIW6tUT/fzvtcHK0SkbQCQXuriHmQ2MQyJX/0kn2NfjLV/ufGxbL1\nmb5qwMGUnEpJaZD6QSSs3kICLwWUYUiGfc0uiSbmJoap/GTLU0W5Mfcv36PaBUNy5p53V6G7hXb2\nbahyWyJNfjLe4M86yd2YK3V2CmK+X/BOsShnJ36+hjrXPPWmV3N9zEmCdJjA+K15DYmhm/tJWSD9\n81oGk9TopEp7CkIfatEATyyZiVqoRq6k64iuM9JkA3OzdXzMQexXVJ1TLZVEH0E7bhlY9d8O1ozR\noQs/FiZNAx2iijCWyv0lpjE73+kCgYEA9mZtyhkHkFDpwrSM1APaL8oNAbbjwEy7Z5Mqfql+lIp1\nYkriL0DbLXlvRAH+yHPRit2hHOjtUNZh4Axv+cpg09qbUI3+43eEy24B7G/Uh+GTfbjsXsOxQx/x\np9otyVwc7hsQ5TA5PZb+mvkJ5OBEKzet9XcKwONBYELGhnEPe7cCgYEA06Vgov6YHleHui9kHuws\nayav0elc5zkxjF9nfHFJRry21R1trw2Vdpn+9g481URrpzWVOEihvm+xTtmaZlSp//lkq75XDwnU\nWA8gkn6O3QE3fq2yN98BURsAKdJfJ5RL1HvGQvTe10HLYYXpJnEkHv+Unl2ajLivWUt5pbBrKbUC\ngYBjbO+OZk0sCcpZ29sbzjYjpIddErySIyRX5gV2uNQwAjLdp9PfN295yQ+BxMBXiIycWVQiw0bH\noMo7yykABY7Ozd5wQewBQ4AdSlWSX4nGDtsiFxWiI5sKuAAeOCbTosy1s8w8fxoJ5Tz1sdoxNeGs\nArq6Wv/G16zQuAE9zK9vvwKBgF+09VI/1wJBirsDGz9whVWfFPrTkJNvJZzYt69qezxlsjgFKshy\nWBhd4xHZtmCqpBPlAymEjr/TOlbxyARmXMnIOWIAnNXMGB4KGSyl1mzSVAoQ+fqR+cJ3d0dyPl1j\njjb0Ed/NY8frlNDxAVHE8BSkdsx2f6ELEyBKJSRr9snRAoGAMrTwYneXzvTskF/S5Fyu0iOegLDa\nNWUH38v/nDCgEpIXD5Hn3qAEcju1IjmbwlvtW+nY2jVhv7UGd8MjwUTNGItdb6nsYqM2asrnF3qS\nVRkAKKKYeGjkpUfVTrW0YFjXkfcrR/V+QFL5OndHAKJXjW7a4ejJLncTzmZSpYzwApc=\n-----END RSA PRIVATE KEY-----", 
         "UserName": "gl-user-remote"
     }
     ```

     When using the AWS CLI, you can automatically generate a properly formatted `.pem` file by including the *\-\-query* and *\-\-output* parameters to your `get-instance-access` request\. 

     To set permissions on the `.pem` file, run the following command:

     ```
     $ chmod 400 MyPrivateKey.pem
     ```

1. **Open a port for the remote connection\.** Instances in Amazon GameLift fleets can only be accessed through ports authorized in the fleet configuration\. You can view a fleet's port settings using the command [https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-port-settings.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/describe-fleet-port-settings.html)\. 

   As a best practice, we recommend opening ports for remote access only when you need them and closing them when you're finished\. Use the command [https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-fleet-port-settings.html) to add a port setting for the remote connection \(such as `22` for SSH or `3389` for RDP\)\. For the IP range value, specify the IP addresses for the devices you plan to use to connect \(converted to CIDR format\)\. Example: 

   ```
   $ aws gamelift update-fleet-port-settings
       --fleet-id  "fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa" 
       --inbound-permission-authorizations "FromPort=22,ToPort=22,IpRange=54.186.139.221/32,Protocol=TCP"
   ```

1. **Open a remote connection client\.** Use Remote Desktop for Windows or SSH for Linux instances\. Connect to the instance using the IP address, port setting, and access credentials\.

   SSH example: 

   ```
   ssh -i MyPrivateKey.pem gl-user-remote@192.0.2.0
   ```

## View and Update Remote Instances<a name="fleets-remote-access-permissions"></a>

When connected to an instance remotely, you have full user and administrative access\. This means you also have the ability to cause errors and failures in game hosting\. If the instance is hosting games with active players, you run the risk of crashing game sessions and dropping players, as well as disrupting game shutdown processes and causing errors in saved game data and logs\.

Hosting resources on an instance can be found in the following locations:
+ **Game build files\.** These are the files included in the game build you uploaded to Amazon GameLift\. They include one or more game server executables, assets and dependencies\. These files are located in a root directory called `game`: 
  + On Windows: `c:\game`
  + On Linux: `/local/game`
+ **Game log files\.** Any log files your game server generates are stored in the `game` root directory at whatever directory path you designated\.
+ **Amazon GameLift hosting resources\.** Files used by the Amazon GameLift service to manage game hosting are located in a root directory called `Whitewater`\. These files should not be changed for any reason\. 
+ **Runtime configuration\.** The fleet runtime configuration is not accessible for individual instances\. To test changes to a runtime configuration \(launch path, launch parameters, maximum number of concurrent processes\), you must update the fleet\-wide runtime configuration \(see the AWS SDK action [UpdateRuntimeConfiguration](https://docs.aws.amazon.com/gamelift/latest/apireference/API_UpdateRuntimeConfiguration.html) or the AWS CLI [update\-runtime\-configuration](https://docs.aws.amazon.com/cli/latest/reference/gamelift/update-runtime-configuration.html)\)\.
+ **TLS certificates\.** If the instance is on a fleet that has TLS certificate generation enabled, certificate files, including the certificate, certificate chain, private key, and root certificate, are stored in the following location:
  + On Windows: `c:\\GameMetadata\Certificates`
  + On Linux: `/local/gamemetadata/certificates/`