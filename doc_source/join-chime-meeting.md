# JoinChimeMeeting<a name="join-chime-meeting"></a>

Join an Amazon Chime SDK meeting by providing the attendee join token\. To do this, you make AWS SDK calls to the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) and [CreateAttendee](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateAttendee.html) APIs to get the token and pass it on in the action\. See the following example\. 

**Note**  
You can't run this action on a bridged call\.

```
{
    "Type": "JoinChimeMeeting",
    "Parameters": {
        "JoinToken": "meeting-attendee-join-token",
        "CallId": "call-id-1"
        "ParticipantTag": "LEG-A"
        "MeetingId": "meeting-id"
    }
}
```

**JoinToken**  
*Description* – A valid join token of the Amazon Chime SDK meeting attendee  
*Allowed values* – Valid join token  
*Required* – Yes  
*Default value* – None

**CallId**  
*Description* – `CallId` of participant in the `CallDetails` of the AWS Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – No  
*Default value* – None

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`  
*Allowed values* – `LEG-A`  
*Required* – No  
*Default value* – `ParticipantTag` of the invoked `callLeg` Ignored if you specify `CallId`

**MeetingId**  
*Description* – A valid Amazon Chime SDK meeting ID associated with the `JoinToken`\. If the meeting was created using an API in the [Amazon Chime](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_Operations_Amazon_Chime.html) namespace, the meeting ID is not required\. If the meeting was created using an API in the [Amazon Chime SDK Meetings](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_Operations_Amazon_Chime_SDK_Meetings.html) namespace, the meeting ID is required\. The meeting is joined using the API endpoint used to create the meeting\.  
*Allowed values* – A valid meeting ID\.  
*Required* – No\.  
*Default value* – None\.

The SIP media application always invokes an AWS Lambda function after running this action\. It returns either the `ACTION_SUCCESSFUL` or `ACTION_FAILED` invocation event types\. The following example shows a successful invocation event structure\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 4,
    "InvocationEvent": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "JoinChimeMeeting",
        "Parameters": {
            "JoinToken": "meeting-attendee-join-token",
            "CallId": "call-id-1"
            "ParticipantTag": "LEG-A"
        }
    }
    "CallDetails": {
        ...
    }
}
```

**Error handling**  
When a validation error occurs while bridging a meeting, the SIP application calls its AWS Lambda function with one of the error messages shown in the following table\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidActionParameter`  |  `JoinToken` parameter value is invalid\.  |  Any of the action's other parameters is invalid or missing\.  | 
|  `SystemException`  |  System error while running action\.  |  Another type of system error occurred while running the action\.  | 

The following example shows a typical failure event\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEvent": "ActionFailed",
    "ActionData": {
        "Type": "JoinChimeMeeting",
        "Parameters": {
            "JoinToken": "meeting-attendee-join-token",
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A"
        },
        "Error": "ErrorJoiningMeeting: Error while joining meeting."
    }
    "CallDetails": {
        ...
    }
}
```

See a working example on GitHub: [https://github\.com/aws\-samples/amazon\-chime\-sma\-update\-call](https://github.com/aws-samples/amazon-chime-sma-update-call)