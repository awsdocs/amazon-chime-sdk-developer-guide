# ModifyChimeMeetingAttendee \(muting and unmuting audio\)<a name="mute-unmute"></a>

Allows the SIP media application to modify the status of a telephony attendee by providing the Amazon Chime meeting ID and attendee list\.

**Note**  
This action currently supports mute and unmute operations on telephony attendees\. Also, the user must be joined into a meeting using the `JoinChimeMeeting` action\. This action can be performed on a `participantTag=“LEG-B”`, or a corresponding `CallId`\. 

This action only applies to the callLeg that joins from the SIP media application to `"+`*13605550122*`"`, LEG\-B, or the leg joined from the SIP media application to the meeting\.

```
{
"SchemaVersion": "1.0",
  "Actions": [
    {
      "Type" : "ModifyChimeMeetingAttendees",
      "Parameters" : {
        "Operation": "Mute",
        "MeetingId": "meeting-id",
        "CallId": "call-id",
        "ParticipantTag": LEG-A",
        "AttendeeList": ["attendee-id-1", "attendee-id-2"]
      }
    }
  ]
}
```

**Operation**  
**Description** – The operation to perform on the list of attendees  
**Allowed values** – Mute, Unmute  
**Required** – Yes  
**Default value** – None

**MeetingId**  
**Description** – The ID of the meeting to which the attendees belong  
**Allowed values** – A valid meeting ID\. The person muting or unmuting must also belong to the meeting\.  
**Required** – Yes  
**Default value** – None

**CallId**  
**Description** – The ID of the meeting to which the attendees belong  
**Allowed values** – A valid call ID\.  
**Required** – No  
**Default value** – None

**ParticipantTag**  
**Description** – The tag assigned to the attendee\.  
**Allowed values** – A valid tag\.  
**Required** – No  
**Default value** – None

**AttendeeList**  
**Description** – List of attendee IDs to mute or unmute  
**Allowed values** – A list of valid attendee IDs  
**Required** – Yes  
**Default value** – None

After running this action, SIP media applications always invoke a Lambda function with the `ACTION_SUCCESSFUL` or `ACTION_FAILED` invocation event type\. The following example code shows a typical `ACTION_SUCCESSFUL` invocation event\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type" : "ModifyChimeMeetingAttendees",
        "Parameters" : {
            "Operation": "Mute",
            "MeetingId": "meeting-id",
            "CallId": "call-id",
            "ParticipantTag": "LEG-A",
            "AttendeeList": ["attendee-id-1", "attendee-id-2"]
        }
    }
    "CallDetails": {
        ...
    }
}
```

**Error handling**  
In cases of invalid instruction parameters or API failures, SIP media applications call a Lambda function with the error message specific to the failed instruction or API\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidActionParameter`  |  The `ModifyChimeMeetingAttendees Operation` parameter value is invalid  |  The `Operation` value must be Mute or Unmute\.  | 
|     |  Meeting ID parameter value is invalid\.  |  Meeting ID is empty\.  | 
|     |  Attendee List parameter value is invalid\.  |  The Attendee ID list is empty\.  | 
|     |  Invalid action on the call\.  |  The call isn't bridged\.  | 
|     |  Call is not connected to Chime Meeting\.  |  The attendee is not connected to a Chime Meeting\.  | 
|     |  One or more attendees are not part of this meeting\. All attendees must be part of this meeting\.  |  The attendee is not authorized to modify attendees in the meeting\.  | 
|  `SystemException`  |  System error while executing action\.  |  A system error occurred while executing an action\.  | 

The following example code shows a typical failure event:

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type" : "ModifyChimeMeetingAttendees",
        "Parameters" : {
            "Operation": "Mute",
            "MeetingId": "meeting-id",
            "CallId": "call-id",
            "ParticipantTag": "LEG-A",
            "AttendeeList": ["attendee-id-1", "attendee-id-2"]
        },
        "ErrorType": "",
        "ErrorMessage": "",
        "ErrorList": []
    }
    "CallDetails": {
        ...
    }
}
```