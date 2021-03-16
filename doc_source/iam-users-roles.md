# Creating IAM users or roles with the Chime SDK policy<a name="iam-users-roles"></a>

You create users as IAM users, or in roles as appropriate to your use case\. You then assign the following policy to them\. This ensures that you have the necessary permissions for the AWS SDK embedded in your server application\. That allows you to perform lifecycle operations on the meeting and attendee resources\. 

```
    // Policy ARN:     arn:aws:iam::aws:policy/AmazonChimeSDK 
    // Description:    Provides access to Amazon Chime SDK operations
    {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "chime:CreateMeeting",
                "chime:DeleteMeeting",
                "chime:GetMeeting",
                "chime:ListMeetings",
                "chime:CreateAttendee",
                "chime:BatchCreateAttendee",
                "chime:DeleteAttendee",
                "chime:GetAttendee",
                "chime:ListAttendees"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]}
```