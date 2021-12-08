# Using echo reduction<a name="use-echo-reduction"></a>

Echo reduction helps keep echoes—sounds from a user’s loudspeaker that get picked up by their microphone—from circulating back into meeting audio and bringing discussions to a standstill\. The echo reduction feature is available in conjunction with the noise reduction provided by Amazon Voice Focus\.

Echo reduction is enabled at the meeting level when you call the [CreateMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateMeeting.html) or [CreateMeetingWithAttendees](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateMeetingWithAttendees.html) APIs\. Enabling the feature this way allows others who join the meeting to enable echo reduction as desired\.

The following example shows a typical request\.

```
POST /meetings HTTP/1.1
Content-type: application/json
{
    "[ClientRequestToken](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html#chime-CreateMeeting-request-ClientRequestToken)": "string",
    "[ExternalMeetingId](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html#chime-CreateMeeting-request-ExternalMeetingId)": "string",
    "[MediaRegion](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html#chime-CreateMeeting-request-MediaRegion)": "string",
    "[MeetingHostId](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html#chime-CreateMeeting-request-MeetingHostId)": "string",
    "[NotificationsConfiguration](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html#chime-CreateMeeting-request-NotificationsConfiguration)": { 
        "[SnsTopicArn](https://docs.aws.amazon.com/chime/latest/APIReference/API_MeetingNotificationConfiguration.html#chime-Type-MeetingNotificationConfiguration-SnsTopicArn)": "string",
        "[SqsQueueArn](https://docs.aws.amazon.com/chime/latest/APIReference/API_MeetingNotificationConfiguration.html#chime-Type-MeetingNotificationConfiguration-SqsQueueArn)": "string"
    },
    "MeetingFeatures": {
        "Audio": {
            "EchoReduction": "AVAILABLE"
        }
    }
}
```

The following example shows a typical response\.

```
HTTP/1.1 201
Content-type: application/json
{
   "[Meeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html#chime-CreateMeeting-response-Meeting)": { 
      "[ExternalMeetingId](https://docs.aws.amazon.com/chime/latest/APIReference/API_Meeting.html#chime-Type-Meeting-ExternalMeetingId)": "*string*",
      "[MediaPlacement](https://docs.aws.amazon.com/chime/latest/APIReference/API_Meeting.html#chime-Type-Meeting-MediaPlacement)": { 
         "[AudioFallbackUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-AudioFallbackUrl)": "*string*",
         "[AudioHostUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-AudioHostUrl)": "*string*",
         "[EventIngestionUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-EventIngestionUrl)": "*string*",
         "[ScreenDataUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-ScreenDataUrl)": "*string*",
         "[ScreenSharingUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-ScreenSharingUrl)": "*string*",
         "[ScreenViewingUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-ScreenViewingUrl)": "*string*",
         "[SignalingUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-SignalingUrl)": "*string*",
         "[TurnControlUrl](https://docs.aws.amazon.com/chime/latest/APIReference/API_MediaPlacement.html#chime-Type-MediaPlacement-TurnControlUrl)": "*string*"
      },
      "[MediaRegion](https://docs.aws.amazon.com/chime/latest/APIReference/API_Meeting.html#chime-Type-Meeting-MediaRegion)": "*string*",
      "[MeetingId](https://docs.aws.amazon.com/chime/latest/APIReference/API_Meeting.html#chime-Type-Meeting-MeetingId)": "*string*",
      "MeetingFeatures": {
         "Audio": {
            "EchoReduction": "string"
         }
      }
   }
}
```

**Understanding the echo reduction process**  
Echo reduction follows this process\.

1. Create a meeting with echo reduction enabled\.

1. The users who join the meeting turn the feature on or off\.

**Creating a meeting with echo reduction enabled**  
Create your meeting by specifying echo reduction when you call the [CreateMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateMeeting.html) or [CreateMeetingWithAttendees](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateMeetingWithAttendees.html) APIs\.

This example shows how to enable echo reduction when calling `CreateMeeting`\.

```
/* Create meeting */
const meetingInfo = await chime.createMeeting({
    ...
   MeetingFeatures: {
        Audio: {
           EchoReduction: 'AVAILABLE' 
       }
   } 
}).promise();

/* Add attendee */
const attendeeInfo = await chime.createAttendee({...});
const joinInfo = { 
    JoinInfo: {
        Meeting: meeting_information,
         Attendee: attendee_information,
    }
}
```

**Enabling echo reduction at the client level**  
 Once you create the meeting with the correct flags, you pass in the `JoinInfo` parameter when creating the Voice Focus device\. In the code sample, note the usage of `ns_es` as the spec name for echo reduction\. Use the default if you want to use Voice Focus without echo reduction\. 

This example shows how to enable echo reduction for meeting attendees\.

```
/* Select the Echo Reduction model */
const spec: VoiceFocusSpec = {
    name: 'ns_es',
    ...
};

/* Create the Voice Focus device */
const transformer = VoiceFocusDeviceTransformer.create(spec, { logger }, joinInfo);

this.audioVideo = this.meetingSession.audioVideo;
const vfDevice = await transformer.createTransformDevice(chosenAudioInput);

/* Enable Echo Reduction on this client */
await vfDevice.observeMeetingAudio(this.audioVideo);
```

**Disabling echo reduction**  
During the meeting, you can disable echo reduction by using the following code\. The code stops echo reduction and maintains the Voice Focus noise reduction\.

```
await vfDevice.unObserveMeetingAudio(this.audioVideo);
```