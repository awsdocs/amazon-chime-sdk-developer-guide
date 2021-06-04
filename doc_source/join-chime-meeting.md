# JoinChimeMeeting<a name="join-chime-meeting"></a>

Join an Amazon Chime SDK meeting by providing the attendee join token\. To do this, you make AWS SDK calls to the [CreateMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html) and [CreateAttendee](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateAttendee.html) APIs to get the token and pass it on in the action\. See the following code example\. 

**Note**  
You can't run this action on a bridged call\.

```
{
    "Type": "JoinChimeMeeting",
    "Parameters": {
        "JoinToken": "meeting-attendee-join-token",
        "CallId": "call-id-1"
    }
}
```

**CallId**  
*Description* – `CallId` of participant in the `CallDetails`  
*Allowed values* – A valid call ID  
*Required* – No, if `ParticipantTag` is present  
*Default value* – None

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`  
*Allowed values* – `LEG-A`  
*Required* – No, if `CallId` is present  
*Default value* – `ParticipantTag` of the invoked `callLeg`\. Ignored if you specify `CallId`\.

**JoinToken**  
*Description* – A valid join token of the Amazon Chime meeting attendee\.   
*Allowed values* – Valid join token  
*Required* – Yes  
*Default value* – None

The SIP media application always invokes a Lambda function after running this action\. It returns either the `ACTION_SUCCESSFUL` or `ActionFailed` invocation event types\. The following code example shows the invocation event structure of the Lambda function\.

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
        }
    }
    "CallDetails": {
        ...
    }
  }
```

**Error handling**  
When a validation error occurs while bridging a meeting, the SIP application calls its Lambda function with one of the error messages shown in the following table\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidActionParameter`  |  `JoinToken` parameter value is invalid\.  |  Any of the action's other parameters is invalid or missing\.  | 
|  `SystemException`  |  System error while running action\.  |  Another type of system error occurred while running the action\.  | 

The following code example shows a typical failure event\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEvent": "ActionFailed",
    "ActionData": {
        "Type": "JoinChimeMeeting",
        "Parameters": {
            "JoinToken": "meeting-attendee-join-token",
            "CallId": "call-id-1"
        },
        "Error": "ErrorJoiningMeeting: Error while joining meeting."
    }
    "CallDetails": {
        ...
    }
}
```