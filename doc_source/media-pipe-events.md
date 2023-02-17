# Using media pipeline events<a name="media-pipe-events"></a>

Each type of media pipeline sends lifecycle events, which you can use to trigger notifications and initiate downstream workflows\. Some examples of using media pipeline events include:
+ Processing captured media after a media pipeline has completed\.
+ Notifying meeting participants if a media pipeline has a temporary failure\.
+ Stopping a meeting if a media pipeline fails permanently\.

You can send events to Amazon EventBridge, Amazon Simple Notification Service \(SNS\), and Amazon Simple Queue Service \(SQS\)\. When sending events to Amazon EventBridge, The Amazon Chime SDK uses *best effort delivery*, meaning the SDK tries to send all events to EventBridge, but in rare cases an event might not be delivered\. For more information, refer to [Events from AWS services](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-service-event.html) in the *Amazon EventBridge User Guide*\.

## Amazon Chime SDK media pipeline created<a name="media-pipeline-create"></a>

The Amazon Chime SDK sends this event when the media pipeline is created\.

**Example: Event data**  
 The following is example data for this event\.

```
{
    "version": "0", 
    "id": "5ee6265a-0a40-104e-d8fd-a3b4bdd78483", 
    "detail-type": "Chime Media Pipeline State Change", 
    "source": "aws.chime", 
    "account": "111122223333", 
    "time": "2021-07-28T20:20:49Z", 
    "region": "us-east-1", 
    "resources": [], 
    "detail": {
        "version": "0", 
        "eventType": "chime:MediaPipelineInProgress", 
        "timestamp": 1627503649251, 
        "meetingId": "1e6bf4f5-f4b5-4917-b8c9-bda45c340706", 
        "externalMeetingId": "Meeting_Id",
        "mediaPipelineId": "e40ee45e-2ed1-408e-9156-f52b8208a491", 
        "mediaRegion": "ap-southeast-1"
    }
}
```

## Amazon Chime SDK media pipeline deleted<a name="media-pipeline-delete"></a>

The Amazon Chime SDK sends this event when the media pipeline is deleted\.

**Example: Event data**  
The following is example data for this event\.

```
{
    "version": "0",
    "id": "9e11e429-97fd-9532-5670-fac3f7abc05f",
    "detail-type": "Chime Media Pipeline State Change",
    "source": "aws.chime",
    "account": "365135496707",
    "time": "2021-07-28T20:21:50Z",
    "region": "us-east-1",
    "resources": [],
    "detail": {
        "version": "0",
        "eventType": "chime:MediaPipelineDeleted",
        "timestamp": 1627503710485,
        "meetingId": "1e6bf4f5-f4b5-4917-b8c9-bda45c340706",
        "externalMeetingId": "Meeting_Id",
        "mediaPipelineId": "e40ee45e-2ed1-408e-9156-f52b8208a491",
        "mediaRegion": "ap-southeast-1"
    }
}
```

## Amazon Chime SDK media pipeline has a temporary failure<a name="pipeline-temp-failure"></a>

The Amazon Chime SDK sends this event when the media pipeline has a temporary failure\.

**Example: Event data**  
The following is example data for this event\.

```
{
    "version": "0",
    "id": "abc141e1-fc2e-65e8-5f18-ab5130f1035a",
    "detail-type": "Chime Media Pipeline State Change",
    "source": "aws.chime",
    "account": "365135496707",
    "time": "2021-07-28T21:16:42Z",
    "region": "us-east-1",
    "resources": [],
    "detail": {
        "version": "0",
        "eventType": "chime:MediaPipelineTemporaryFailure",
        "timestamp": 1627507002882,
        "meetingId": "7a5434e3-724a-4bbb-9eb6-2fb209dc0706",
        "externalMeetingId": "Meeting_Id",
        "mediaPipelineId": "ebd62f4e-04a9-426d-bcb0-974c0f266400",
        "mediaRegion": "eu-south-1"
    }
}
```

## Amazon Chime SDK media pipeline resumes after a temporary failure<a name="pipeline-temp-failure-resume"></a>

The Amazon Chime SDK sends this event when the media pipeline resumes after a temporary failure\.

**Example: Event data**  
The following is example data for this event\.

```
{
    "version": "0",
    "id": "9e11e429-97fd-9532-5670-fac3f7abc05f",
    "detail-type": "Chime Media Pipeline State Change",
    "source": "aws.chime",
    "account": "365135496707",
    "time": "2021-07-28T20:21:50Z",
    "region": "us-east-1",
    "resources": [],
    "detail": {
        "version": "0",
        "eventType": "chime:MediaPipelineResumed",
        "timestamp": 1627503710485?,
        "meetingId": "1e6bf4f5-f4b5-4917-b8c9-bda45c340706",
        "externalMeetingId": "Meeting_Id",
        "mediaPipelineId": "e40ee45e-2ed1-408e-9156-f52b8208a491",
        "mediaRegion": "ap-southeast-1"
    }
}
```

## Amazon Chime SDK media pipeline permanent failure<a name="pipeline-perm-failure"></a>

The Amazon Chime SDK sends this event when a media pipeline fails permanently\.

**Example: Event data**  
The following is example data for this event\.

```
{
    "version": "0",
    "id": "9e11e429-97fd-9532-5670-fac3f7abc05f",
    "detail-type": "Chime Media Pipeline State Change",
    "source": "aws.chime",
    "account": "365135496707",
    "time": "2021-07-28T20:21:50Z",
    "region": "us-east-1",
    "resources": [],
    "detail": {
        "version": "0",
        "eventType": "chime:MediaPipelinePermanentFailure",
        "timestamp": 1627503710485,
        "meetingId": "1e6bf4f5-f4b5-4917-b8c9-bda45c340706",
        "externalMeetingId": "Meeting_Id",
        "mediaPipelineId": "e40ee45e-2ed1-408e-9156-f52b8208a491",
        "mediaRegion": "ap-southeast-1"
    }
}
```