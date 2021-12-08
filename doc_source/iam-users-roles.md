# Creating IAM users or roles with the Chime SDK policy<a name="iam-users-roles"></a>

You create users as IAM users, or in roles as appropriate to your use case\. You then assign the following policy to them\. This ensures that you have the necessary permissions for the AWS SDK embedded in your server application\. That allows you to perform lifecycle operations on the meeting and attendee resources\. 

This example shows the current version of the AWSChimeSDK AWS managed policy\. It grants access to the Amazon Chime SDK operations\.

```
// Policy ARN: arn:aws:iam::aws:policy/AmazonChimeSDK 
// Description: Provides access to Amazon Chime SDK operations
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "chime:CreateMeeting",
                "chime:CreateMeetingWithAttendees",
                "chime:DeleteMeeting",
                "chime:GetMeeting",
                "chime:ListMeetings",
                "chime:CreateAttendee",
                "chime:BatchCreateAttendee",
                "chime:DeleteAttendee",
                "chime:GetAttendee",
                "chime:ListAttendees",
                "chime:ListAttendeeTags",
                "chime:ListMeetingTags",
                "chime:ListTagsForResource",
                "chime:TagAttendee",
                "chime:TagMeeting",
                "chime:TagResource",
                "chime:UntagAttendee",
                "chime:UntagMeeting",
                "chime:UntagResource",
                "chime:StartMeetingTranscription",
                "chime:StopMeetingTranscription"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

## SDK policy updates<a name="pol-changes"></a>

This table lists and describes the updates to the Amazon Chime IAM policy\.


| Change | Description | Date | 
| --- | --- | --- | 
| Added support for transcriptions\. | Amazon Chime added the `StartMeetingTranscription` and `StopMeetingTranscription` actions to grant access to transcription services\. | September 9, 2021 | 
| Amazon Chime started tracking changes\. | Amazon Chime started tracking changes to the AWSChimeSDK AWS managed policy\. | September 9, 2021 | 