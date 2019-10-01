# Integrating a Game Client for Realtime Servers<a name="realtime-client"></a>

This topic describes how to prepare your game client to be able to join and participate in Amazon GameLift\-hosted game sessions\.

There are two sets of tasks needed to prepare your game client: 
+ Set up your game client to acquire information about existing games, request matchmaking, start new game sessions, and reserve game session slots for a player\. 
+ Enable your game client to join a game session hosted on a Realtime server and exchange messages\. 

## Find or Create Game Sessions and Player Sessions<a name="realtime-client-start-gamesession"></a>

Set up your game client to find or start game sessions, request FlexMatch matchmaking, and reserve space for players in a game by creating player sessions\. As a best practice, create a client service and use it to make the direct requests to the Amazon GameLift service when triggered by a game client action\. The client service then relays relevant responses back to the game client\. 

1. Add the AWS SDK to your client service project, initialize an Amazon GameLift client, and configure it to use the hosting resources in your fleets and/or queues\. The AWS SDK is available in several languages; see the Amazon GameLift SDKs [For Client Services](gamelift-supported.md#gamelift-supported-clients)\. 

1. Add GameLift functionality to your client service\. For more detailed instructions, see [Add Amazon GameLift to Your Game Client](gamelift-sdk-client-api.md) and [Adding FlexMatch Matchmaking](match-intro.md)\. The best practice is to use game session placements to create new game sessions\. This method lets you take full advantage of GameLift's ability to quickly and intelligently place new game sessions, as well as use player latency data to minimize game lag\. At a minimum, your client service must be able to request new game sessions and handle game session data in response\. You may also want to add functionality to search for and get information on existing game sessions, and request player sessions, which effectively reserve a player slot in an existing game session\.

1. Convey connection information back to the game client\. The backend game service receives game session and player session objects in response to requests to the Amazon GameLift service\. These objects contain information, in particular connection details \(IP address and port\) and player session ID, that the game client needs to connect to the game session running on a Realtime Server\. 

## Connect to Games on Realtime Servers<a name="realtime-client-connect"></a>

 Enable your game client to connect directly with a hosted game session on a Realtime server and exchange messages with the server and with other players\.

1. Get the Realtime Client SDK, build it, and add it to your game client project\. See the README file for more information on SDK requirements and instructions on how to build the client libraries\. 

1. Call [Client\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-client) with a client configuration that specifies the type of client/server connection to use\.
**Note**  
If you're connecting to a Realtime server that is running on a secured fleet with a TLS certificate, you must specify a secured connection type\.

1. Add the following functionality to your game client\. See the [Realtime Servers Client API \(C\#\) Reference](realtime-sdk-csharp-ref.md) for more information\. 
   + Connect to and disconnect from a game 
     + [Connect\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-connect)
     + [Disconnect\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-disconnect)
   + Send messages to target recipients 
     + [SendMessage\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-sendmessage)
   + Receive and process messages 
     + [OnDataReceived\(\)](realtime-sdk-csharp-ref-callbacks.md#realtime-sdk-csharp-ref-callbacks-ondata)
   + Join groups and leave player groups 
     + [JoinGroup\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-joingroup)
     + [RequestGroupMembership\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-requestgroupmembership)
     + [LeaveGroup\(\)](realtime-sdk-csharp-ref-actions.md#realtime-sdk-csharp-ref-actions-leavegroup)

1. Set up event handlers for the client callbacks as needed\. See [Realtime Servers Client API \(C\#\) Reference: Asynchronous Callbacks](realtime-sdk-csharp-ref-callbacks.md)\.

When working with Realtime fleets that have TLS certificate generation enabled, the server is automatically authenticated using the TLS certificate\. TCP and UDP traffic is encrypted in flight to provide transport layer security\. TCP traffic is encrypted using TLS 1\.2, and UDP traffic is encrypted using DTLS 1\.2\.

## Game Client Examples<a name="realtime-client-examples"></a>

### Basic Realtime Client \(C\#\)<a name="realtime-client-examples-basic"></a>

This example illustrates a basic game client integration with the Realtime Client SDK \(C\#\)\. As shown, the example initializes a Realtime client object, sets up event handlers and implements the client\-side callbacks, connects to a Realtime server, sends a message, and disconnects\.

```
using System;
using System.Text;
using Aws.GameLift.Realtime;
using Aws.GameLift.Realtime.Event;
using Aws.GameLift.Realtime.Types;

namespace Example
{
    /**
     * An example client that wraps the GameLift Realtime client SDK
     * 
     * You can redirect logging from the SDK by setting up the LogHandler as such:
     * ClientLogger.LogHandler = (x) => Console.WriteLine(x);
     *
     */
    class RealTimeClient
    {
        public Aws.GameLift.Realtime.Client Client { get; private set; }
        
        // An opcode defined by client and your server script that represents a custom message type
        private const int MY_TEST_OP_CODE = 10;

        /// Initialize a client for GameLift Realtime and connect to a player session.
        /// <param name="endpoint">The DNS name that is assigned to Realtime server</param>
        /// <param name="remoteTcpPort">A TCP port for the Realtime server</param>
        /// <param name="listeningUdpPort">A local port for listening to UDP traffic</param>
        /// <param name="connectionType">Type of connection to establish between client and the Realtime server</param>
        /// <param name="playerSessionId">The player session ID that is assiged to the game client for a game session </param>
        /// <param name="connectionPayload">Developer-defined data to be used during client connection, such as for player authentication</param>
       public RealTimeClient(string endpoint, int remoteTcpPort, int listeningUdpPort, ConnectionType connectionType,
                    string playerSessionId, byte[] connectionPayload)
        {
            // Create a client configuration to specify a secure or unsecure connection type
            // Best practice is to set up a secure connection using the connection type RT_OVER_WSS_DTLS_TLS12.
            ClientConfiguration clientConfiguration = new ClientConfiguration()
	     {
                // C# notation to set the field ConnectionType in the new instance of ClientConfiguration
	         ConnectionType = connectionType
	     };

            // Create a Realtime client with the client configuration            
            Client = new Client(clientConfiguration);

            // Initialize event handlers for the Realtime client
            Client.ConnectionOpen += OnOpenEvent;
            Client.ConnectionClose += OnCloseEvent;
            Client.GroupMembershipUpdated += OnGroupMembershipUpdate;
            Client.DataReceived += OnDataReceived;

            // Create a connection token to authenticate the client with the Realtime server
            // Player session IDs can be retrieved using AWS SDK for GameLift
            ConnectionToken connectionToken = new ConnectionToken(playerSessionId, connectionPayload);

            // Initiate a connection with the Realtime server with the given connection information
            Client.Connect(endpoint, remoteTcpPort, listeningUdpPort, connectionToken);
        }

        public void Disconnect()
        {
            if (Client.Connected)
            {
                Client.Disconnect();
            }
        }

        public bool IsConnected()
        {
            return Client.Connected;
        }

        /// <summary>
        /// Example of sending to a custom message to the server.
        /// 
        /// Server could be replaced by known peer Id etc.
        /// </summary>
        /// <param name="intent">Choice of delivery intent ie Reliable, Fast etc. </param>
        /// <param name="payload">Custom payload to send with message</param>
        public void SendMessage(DeliveryIntent intent, string payload)
        {
            Client.SendMessage(Client.NewMessage(MY_TEST_OP_CODE)
                .WithDeliveryIntent(intent)
                .WithTargetPlayer(Constants.PLAYER_ID_SERVER)
                .WithPayload(StringToBytes(payload)));
        }

        /**
         * Handle connection open events
         */
        public void OnOpenEvent(object sender, EventArgs e)
        {
        }

        /**
         * Handle connection close events
         */
        public void OnCloseEvent(object sender, EventArgs e)
        {
        }

        /**
         * Handle Group membership update events 
         */
        public void OnGroupMembershipUpdate(object sender, GroupMembershipEventArgs e)
        {
        }

        /**
         *  Handle data received from the Realtime server 
         */
        public virtual void OnDataReceived(object sender, DataReceivedEventArgs e)
        {
            switch (e.OpCode)
            {
                // handle message based on OpCode
                default:
                    break;
            }
        }

        /**
         * Helper method to simplify task of sending/receiving payloads.
         */
        public static byte[] StringToBytes(string str)
        {
            return Encoding.UTF8.GetBytes(str);
        }

        /**
         * Helper method to simplify task of sending/receiving payloads.
         */
        public static string BytesToString(byte[] bytes)
        {
            return Encoding.UTF8.GetString(bytes);
        }
    }
}
```