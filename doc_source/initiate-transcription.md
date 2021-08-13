# Starting and stopping transcription<a name="initiate-transcription"></a>

You use the Amazon Chime [StartMeetingTranscription](https://docs.aws.amazon.com/chime/latest/APIReference/API_StartMeetingTranscription.html) API to initiate meeting transcription by applying a `TranscriptionConfiguration` to the meeting\. The Amazon Chime controller forwards the configuration to the meeting asynchronously\. The success or failure of initiating meeting transcription is signaled through a message via Amazon Simple Notification Service \(Amazon SNS\) and Amazon EventBridge\.

**Starting transcription**  


This example shows how to start live transcription with Amazon Transcribe\.

```
POST /meetings/meetingId/transcription?operation=start HTTP/1.1 
Content-type: application/json
{
    "TranscriptionConfiguration": {
        "EngineTranscribeSettings": {
            "[LanguageCode](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_ResponseSyntax)": "en-US",  
            "[VocabularyFilterMethod](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_ResponseSyntax)": "tag",
            "[VocabularyFilterName](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_RequestSyntax)": "profanity",
            "[VocabularyName](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_RequestSyntax)": "lingo",
            "Region": "us-east-1"
        }
    }
}
```

This example shows how to star live transcription with Amazon Transcribe Medical\.

```
POST /meetings/meetingId/transcription?operation=start HTTP/1.1 
Content-type: application/json
{  
    "TranscriptionConfiguration": {
        "EngineTranscribeMedicalSettings": {
            "[LanguageCode](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_ResponseSyntax)": "en-US",
            "[Specialty](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_RequestSyntax)": "PRIMARYCARE",
            "[Type](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_RequestSyntax)": "CONVERSATION",
            "[VocabularyName](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_RequestSyntax)": "lingo",
            "Region": "us-east-1"
        }
   }
}
```

**StartMeetingTranscription** – Starts transcription for the meeting\.  
*meetingId* – The ID of the meeting, returned by the [CreateMeeting API](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html#API_CreateMeeting_ResponseSyntax)\.  
*TranscriptionConfiguration* – Encapsulates the parameters for live transcription\. You must specifiy exactly one configuration, `EngineTranscribeSettings` or `EngineTranscribeMedicalSettings`\.

**EngineTranscribeSettings** – Specifies the use of Amazon Transcribe and passes its settings through to [ Amazon Transcribe](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html#API_streaming_StartStreamTranscription_RequestParameters)\.  
*LanguageCode* – Required\.  
*VocabularyFilterMethod* – Optional\.  
*VocabularyFilterName* – Optional\.  
*VocabularyName* – Optional\.  
*Region* – Optional\.

**EngineTranscribeMedicalSettings** – Specifies the use of Amazon Transcribe Medical and passes its settings through to [ Amazon Transcribe Medical](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartMedicalStreamTranscription.html#API_streaming_StartMedicalStreamTranscription_RequestParameters)\.   
*LanguageCode* – Required\.  
*Speciality* – Required\.  
*Type* – Required\.  
*VocabularyName* – Optional\.  
*Region* – Optional\.

**Responses**  
Amazon Transcribe and Amazon Transcribe Medical take the following responses:
+ `OK` \(200\) with empty body, if you successfully apply the `TranscriptionConfiguration` to the meeting\.

**Error messages**  
Amazon Transcribe and Amazon Transcribe Medical display the following error messages:
+ `BadRequestException (400):` The input parameters don't match the service's restrictions\.
+ `ForbiddenException (403):` The client is permanently forbidden from making the request\.
+ `NotFoundException (404):` The `meetingId` does not exist\.
+ `ResourceLimitExceededException (400):` The request exceeds the resource limit\. For example, too many meetings have live transcription enabled\.
+ `ServiceFailureException (500):` The service encountered an unexpected error\.
+ `ServiceUnavailableException (503):` The service is currently unavailable\.
+ `ThrottledClientException (429):` The client exceeded its request rate limit\.
+ `UnauthorizedClientException (401):` The client is not currently authorized to make the request\.

Calling `StartMeetingTranscription` a second time updates the `TranscriptionConfiguration` applied to the meeting\.

**Stopping transcription**  
You use the [StopMeetingTranscription](https://docs.aws.amazon.com/chime/latest/APIReference/API_StopMeetingTranscription.html) API to remove the `TranscriptionConfiguration` for a given `meetingID` and end meeting transcription\. Ending a meeting stops transcription automatically\.

This example shows the request syntax that invokes `StopMeetingTranscription`\.

```
POST/meetings/meetingId/transcription?operation=stop HTTP/1.1
```

**Responses**  
Amazon Transcribe and Amazon Transcribe Medical take the following responses:
+ `OK` \(200\) with empty body, if you successfully remove the `TranscriptionConfiguration` from the meeting\.

**Error messages**  
Amazon Transcribe and Amazon Transcribe Medical display the following error messages:
+ `BadRequestException (400):` The input parameters don't match the service's restrictions\.
+ `ForbiddenException (403):` The client is permanently forbidden from making the request\.
+ `NotFoundException (404):` The `meetingId` does not exist\.
+ `ServiceFailureException (500):` The service encountered an unexpected error\.
+ `ServiceUnavailableException (503):` The service is currently unavailable\.
+ `ThrottledClientException (429):` The client exceeded its request rate limit\.
+ `UnauthorizedClientException (401):` The client is not currently authorized to make the request\.