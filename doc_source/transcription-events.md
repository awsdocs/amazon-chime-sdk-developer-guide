# Transcription events<a name="transcription-events"></a>

Whenever the status of transcription in a meeting changes, EventBridge and Amazon SNS emit one of the following events to every connected client:

1. `TranscriptionStarted` – a `TranscriptionConfiguration` is applied to a meeting, or updated\.

1. `TranscriptionStopped` – a `TranscriptionConfiguration` is removed from a meeting\.

1. `TranscriptionInterrupted` – connectivity to Amazon Transcribe or Amazon Transcribe Medical has been temporarily interrupted\.

1. `TranscriptionResumed` – connectivity to Amazon Transcribe or Amazon Transcribe Medical was restored after a temporary interruption\.

1. `TranscriptionFailed` – connectivity Amazon Transcribe or Amazon Transcribe Medical has failed and cannot be resumed\.

`TranscriptionInterrupted` and `TranscriptionFailed` include a reason for the interruption or failure\. This example shows the schema for a `TranscriptionInterrupted` event:

```
{
    "version": "0",
    "source": "aws.chime",
    "account": "111122223333",
    "id": "12345678-1234-1234-1234-111122223333",
    "region": "us-east-1",
    "detail-type": "Meeting State Change",
    "time": "yyyy-mm-ddThh:mm:ssZ",
    "resources": [],
    "detail": {
        "version": "0",
        "eventType": "chime:TranscriptionInterrupted",
        "eventTime": 12344566754,
        "meetingId": "87654321-4321-4321-1234-111122223333",
        "message": "Internal server error",
        "transcriptionRegion": "us-east-1",
        "transcriptionConfig" : "{...}",
    }
}
```