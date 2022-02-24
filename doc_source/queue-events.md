# Game session placement events<a name="queue-events"></a>

GameLift emits events for each game session placement request as it is processed\. You can publish these events to an Amazon SNS topic, as described in [Set up event notification for game session placement](queue-notification.md)\. These events are also emitted to Amazon CloudWatch Events in near real time and on a best\-effort basis\.

This topic describes the structure of game session placement events and provides an example for each event type\. For more information on the status of game session placement requests, see [GameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_GameSessionPlacement.html) in the *Amazon GameLift API Reference*\.

## Placement event syntax<a name="queue-events-header"></a>

Events are represented as JSON objects\. Event structure conforms to the CloudWatch Events pattern, with similar top\-level fields and service\-specific details\. 

Top\-level fields include the following \(see [ event pattern](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatchEventsandEventPatterns.html) for more detail\): 

version  
This field is always set to 0 \(zero\)\.

id  
Unique tracking identifier for the event\.

detail\-type  
Value is always `GameLift Queue Placement Event`\.

source  
Value is always `aws.gamelift`\.

account  
The AWS account that is being used to manage GameLift\.

time  
Event timestamp\.

region  
The AWS Region where the placement request is being processed\. This is the Region where the game session queue in use resides\.

resources  
ARN value of the game session queue that is processing the placement request\.

## PlacementFulfilled<a name="queue-events-placementfulfilled"></a>

The placement request has been successfully fulfilled\. A new game session has been started and new player sessions have been created for each player listed in the game session placement request\. Player connection information is available\.

**Detail syntax: **

placementId  
A unique identifier assigned to the game session placement request\.

port  
The port number for the new game session\. 

gameSessionArn  
The ARN identifier for the new game session\. 

ipAddress  
The IP address of the game session\.

dnsName  
The DNS identifier assigned to the instance that is running the new game session\. The value format is different depending on whether the instance running the game session is TLS\-enabled\. When connecting to a game session on a TLS\-enabled fleet, players must use the DNS name, not the IP address\.  
TLS\-enabled fleets: `<unique identifier>.<region identifier>.amazongamelift.com`\.   
Non\-TLS\-enabled fleets: `ec2-<unique identifier>.compute.amazonaws.com`\. 

startTime  
Time stamp indicating when this request was placed in the queue\.

endTime  
Time stamp indicating when this request was fulfilled\.

gameSessionRegion  
AWS Region where the game session is being hosted\. This information is also in the gameSessionArn value\.

placedPlayerSessions  
The collection of player sessions that have been created for each player in the game session placement request\. 

### Example<a name="queue-events-placementfulfilled-example"></a>

```
{
  "version": "0",
  "id": "1111aaaa-bb22-cc33-dd44-5555eeee66ff",
  "detail-type": "GameLift Queue Placement Event",
  "source": "aws.gamelift",
  "account": "123456789012",
  "time": "2021-03-01T15:50:52Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:gamelift:us-west-2:123456789012:gamesessionqueue/MegaFrogRace-NA"
  ],
  "detail": {
    "type": "PlacementFulfilled",
    "placementId": "9999ffff-88ee-77dd-66cc-5555bb44aa",
    "port": "6262",
    "gameSessionArn": "arn:aws:gamelift:us-west-2::gamesession/fleet-2222bbbb-33cc-44dd-55ee-6666ffff77aa/4444dddd-55ee-66ff-77aa-8888bbbb99cc",
    "ipAddress": "98.987.98.987",
    "dnsName": "ec2-12-345-67-890.us-west-2.compute.amazonaws.com",
    "startTime": "2021-03-01T15:50:49.741Z",
    "endTime": "2021-03-01T15:50:52.084Z",
    "gameSessionRegion": "us-west-2",
    "placedPlayerSessions": [
      {
        "playerId": "player-1"
        "playerSessionId": "psess-1232131232324124123123"
      }
    ]
  }
}
```

## PlacementCancelled<a name="queue-events-placementcancelled"></a>

The placement request was canceled with a call to the GameLift service [StopGameSessionPlacement](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StopGameSessionPlacement.html)\.

**Detail:**

placementId  
A unique identifier assigned to the game session placement request\.

startTime  
Time stamp indicating when this request was placed in the queue\.

endTime  
Time stamp indicating when this request was cancelled\. 

### Example<a name="queue-events-placementcancelled-example"></a>

```
{
  "version": "0",
  "id": "1111aaaa-bb22-cc33-dd44-5555eeee66ff",
  "detail-type": "GameLift Queue Placement Event",
  "source": "aws.gamelift",
  "account": "123456789012",
  "time": "2021-03-01T15:50:52Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:gamelift:us-west-2:123456789012:gamesessionqueue/MegaFrogRace-NA"
  ],
  "detail": {

    "type": "PlacementCancelled",
    "placementId": "9999ffff-88ee-77dd-66cc-5555bb44aa",
    "startTime": "2021-03-01T15:50:49.741Z",
    "endTime": "2021-03-01T15:50:52.084Z"
  }
}
```

## PlacementTimedOut<a name="queue-events-placementtimedout"></a>

Game session placement did not successfully complete before the queue's time limit expired\. The placement request can be resubmitted as needed\.

**Detail:** 

placementId  
A unique identifier assigned to the game session placement request\.

startTime  
Time stamp indicating when this request was placed in the queue\.

endTime  
Time stamp indicating when this request was cancelled\. 

### Example<a name="queue-events-placementtimedout-example"></a>

```
{
  "version": "0",
  "id": "1111aaaa-bb22-cc33-dd44-5555eeee66ff",
  "detail-type": "GameLift Queue Placement Event",
  "source": "aws.gamelift",
  "account": "123456789012",
  "time": "2021-03-01T15:50:52Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:gamelift:us-west-2:123456789012:gamesessionqueue/MegaFrogRace-NA"
  ],
  "detail": {

    "type": "PlacementTimedOut",
    "placementId": "9999ffff-88ee-77dd-66cc-5555bb44aa",
    "startTime": "2021-03-01T15:50:49.741Z",
    "endTime": "2021-03-01T15:50:52.084Z"
  }
}
```

## PlacementFailed<a name="queue-events-placementfailed"></a>

GameLift was not able to fulfill the game session request\. This is generally caused by an unexpected internal error\. The placement request can be resubmitted as needed\.

**Detail:**

placementId  
A unique identifier assigned to the game session placement request\.

startTime  
Time stamp indicating when this request was placed in the queue\.

endTime  
Time stamp indicating when this request failed\. 

### Example<a name="queue-events-placementfailed-example"></a>

```
{
  "version": "0",
  "id": "39c978f3-ba46-3f7c-e787-55bfcca1bd31",
  "detail-type": "GameLift Queue Placement Event",
  "source": "aws.gamelift",
  "account": "252386620677",
  "time": "2021-03-01T15:50:52Z",
  "region": "us-east-1",
  "resources": [
    "arn:aws:gamelift:us-west-2:252386620677:gamesessionqueue/MegaFrogRace-NA"
  ],
  "detail": {

    "type": "PlacementFailed",
    "placementId": "e4a1119a-39af-45cf-a990-ef150fe0d453",
    "startTime": "2021-03-01T15:50:49.741Z",
    "endTime": "2021-03-01T15:50:52.084Z"
  }
}
```