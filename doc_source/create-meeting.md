# Creating a meeting<a name="create-meeting"></a>

A CreateMeeting API call accepts a required parameter, the ClientRequestToken, that allows developers to pass in a uniqueness context\. It also accepts optional parameters such as MediaRegion, which represents the media services data plane region to chose for the meeting, the MeetingHostId used to pass in an opaque identifier to represent the meeting host, if applicable, and the NotificationsConfiguration for receiving meeting lifecycle events\. By default, Amazon EventBridge delivers the events\. Optionally, you can also receive events by passing an SQS queue ARN or an SNS Topic ARN in NotificationsConfiguration\. The API Returns a Meeting object that contains a unique MeetingId, the MediaRegion and the MediaPlacement object with a set of media URLs\.

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