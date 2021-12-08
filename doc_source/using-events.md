# Meeting events<a name="using-events"></a>

The Amazon Chime SDK sends meeting lifecycle events, which you can use to trigger notifications and initiate downstream workflows\. Some examples of using meeting events include: 
+ Updating metadata when an attendee joins or leaves an Amazon Chime SDK meeting\.
+ Implementing push notifications or rosters for an Amazon Chime SDK meeting\.
+ Measuring the usage of video and content sharing in Amazon Chime SDK meetings\.

You can send events to Amazon EventBridge, Amazon Simple Notification Service \(SNS\), and Amazon Simple Queue Service \(SQS\)\. When sending events to Amazon EventBridge, The Amazon Chime SDK uses *best effort delivery*, meaning the SDK tries to send all events to EventBridge, but in rare cases an event might not be delivered\. For more information, refer to [Events from AWS services](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-service-event.html) in the *Amazon EventBridge User Guide*\.

## Amazon Chime SDK meeting starts<a name="sdk-start-mtg"></a>

The Amazon Chime SDK sends this event when a new meeting starts\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:MeetingStarted",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "externalMeetingId": "mymeeting",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK meeting ends<a name="sdk-end-mtg"></a>

The Amazon Chime SDK sends this event when an active meeting ends\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:MeetingEnded",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "externalMeetingId": "mymeeting",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee is added<a name="sdk-add-attendee"></a>

The Amazon Chime SDK sends this event when a new attendee is added to an active meeting\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeAdded",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee is removed<a name="sdk-remove-attendee"></a>

The Amazon Chime SDK sends this event when an attendee is removed from an active meeting\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeDeleted",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee is authorized<a name="sdk-auth-attendee"></a>

The Amazon Chime SDK sends this event when an existing attendee joins a meeting\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeAuthorized",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee joins a meeting<a name="sdk-join-attendee"></a>

The Amazon Chime SDK sends this event when an existing attendee joins an Amazon Chime SDK meeting using the specified network transport\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeJoined",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",    
    "networkType": "Voip",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee leaves a meeting<a name="sdk-leave-attendee"></a>

The Amazon Chime SDK sends this event when an existing attendee leaves an Amazon Chime SDK meeting using the specified network transport\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeLeft",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "networkType": "Voip",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee drops from a meeting<a name="sdk-drop-attendee"></a>

The Amazon Chime SDK sends this event when an existing attendee drops from an Amazon Chime SDK meeting using the specified network transport\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeDropped",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "networkType": "Voip",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee starts streaming video<a name="sdk-attendee-video-start"></a>

The Amazon Chime SDK sends this event when an existing attendee starts streaming video\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeVideoStarted",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee stops streaming video<a name="sdk-attendee-video-stop"></a>

The Amazon Chime SDK sends this event when an existing attendee stops streaming video\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeVideoStopped",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee starts sharing screen<a name="sdk-attendee-screenshare-start"></a>

The Amazon Chime SDK sends this event when an existing attendee starts sharing their screen\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeContentJoined",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee stops sharing screen<a name="sdk-attendee-screenshare-stop"></a>

The Amazon Chime SDK sends this event when an existing attendee stops sharing their screen\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeContentLeft",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee content joins a meeting<a name="sdk-content-join"></a>

The Amazon Chime SDK sends this event when a content share joins an Amazon Chime SDK meeting using the specified network transport\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeContentJoined",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "networkType": "Voip",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee content leaves a meeting<a name="sdk-content-leave"></a>

The Amazon Chime SDK sends this event when a content share leaves an Amazon Chime SDK meeting using the specified network transport\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeContentLeft",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "networkType": "Voip",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee content drops from a meeting<a name="sdk-content-drop"></a>

The Amazon Chime SDK sends this event when a content share drops from an Amazon Chime SDK meeting using the specified network transport\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeContentDropped",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "networkType": "Voip",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee content starts streaming video<a name="sdk-content-start-stream"></a>

The Amazon Chime SDK sends this event when a content share starts streaming video\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeContentVideoStarted",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```

## Amazon Chime SDK attendee content stops streaming video<a name="sdk-content-stop-stream"></a>

The Amazon Chime SDK sends this event when a content share stops streaming video\.

**Example : Event data**  
The following is example data for this event\.  

```
{
  "version": "0",
  "source": "aws.chime",
  "account": "111122223333",
  "id": "12345678-1234-1234-1234-111122223333",
  "region": "us-east-1",
  "detail-type": "Chime Meeting State Change",
  "time": "yyyy-mm-ddThh:mm:ssZ",
  "resources": []
  "detail": {
    "version": "0",
    "eventType": "chime:AttendeeContentVideoStopped",
    "timestamp": 12344566754,
    "meetingId": "87654321-4321-4321-1234-111122223333",
    "attendeeId": "87654321-4321-4321-1234-111122223333",
    "externalUserId": "87654321-4321-4321-1234-111122223333",
    "mediaRegion": "us-east-1"
  }
}
```