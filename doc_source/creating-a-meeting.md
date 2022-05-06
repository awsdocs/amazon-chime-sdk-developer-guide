# Creating a meeting<a name="creating-a-meeting"></a>

You use the `CreateMeeting` and `CreateMeetingWithAttendee` API calls to create meetings\. The calls have one required parameter, `ClientRequestToken`\. When two or more users try to create the same meeting, the parameter ensures that Amazon Chime SDK only creates one meeting\. You need to pass in a meeting id, or a unique hash of the id\. The Amazon Chime SDK service handles the race condition of simultaneous calls—any combination of C`CreateMeeting` and `CreateMeetingWithAttendee`—and guarantees that only one meeting exists for the given token\.

The APIs also accept optional parameters such as `MediaRegion`, which represents the media services data plane region to chose for the meeting, the `MeetingHostId` used to pass in an opaque identifier to represent the meeting host, if applicable, and the `NotificationsConfiguration` for receiving meeting lifecycle events\. By default,Amazon EventBridge delivers the events\. Events are emitted on a best\-effort basis for Amazon Chime SDK events\.

Optionally, you can also receive events by passing an SQS queue ARN or an SNS Topic ARN in the `NotificationsConfiguration` parameter\. The API Returns a `Meeting` object that contains a unique `MeetingId`, the `MediaRegion`, and the `MediaPlacement` object with a set of media URLs\. 

```
    meeting = await chime.createMeeting({
                ClientRequestToken: clientRequestToken,
                MediaRegion: mediaRegion,
                MeetingHostId: meetingHostId,
                NotificationsConfiguration: {
                   SqsQueueArn: sqsQueueArn,
                   SnsTopicArn: snsTopicArn
                }
            }).promise();
```