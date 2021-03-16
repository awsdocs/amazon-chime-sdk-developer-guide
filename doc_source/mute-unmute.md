# ModifyChimeMeetingAttendees \(muting and unmuting audio\)<a name="mute-unmute"></a>

Allows the SIP application to modify the status of telephony attendees joined in the meeting by providing the Amazon Chime meeting ID and attendee list\.

**Note**  
This action currently supports mute and unmute operations on telephony attendees\. Also, the current user must be joined into a meeting using the `JoinChimeMeeting` action\.

```
{
"SchemaVersion": "1.0",
  "Actions": [
    {
      "Type" : "ModifyChimeMeetingAttendees",
      "Parameters" : {
        "Operation": "Mute", //or Unmute
        "MeetingId": "meeting-id",
        "AttendeeList": ["attendee-id-1", "attendee-id-2"]
      }
    }
  ]
}
```

Operation  
*Description* – The operation to perform on the list of attendees  
*Allowed Values* – Mute, Unmute  
*Required* – Yes  
*Default value* – None

MeetingId  
*Description* – The ID of the meeting to which the attendees belong\.  
*Allowed Values* – A valid meeting ID\. The person muting or unmuting must also belong to the meeting\.  
*Required* – Yes  
*Default value* – None

AttendeeList  
*Description* – List of attendee IDs to mute or unmute  
*Allowed Values* – A list of valid attendee IDs  
*Required* – Yes  
*Default value* – None

After running this action, SIP media applications always invoke a Lambda function with the `ACTION_SUCCESSFUL` or `ACTION_FAILED` invocation event type\. The following example shows a typical invocation event structure for a Lambda function\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type" : "ModifyChimeMeetingAttendees",
        "Parameters" : {
            "Operation": "Mute", //or Unmute
            "MeetingId": "meeting-id",
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


| Error | Message | Reason | 
| --- | --- | --- | 
| `InvalidActionParameter` | The `ModifyChimeMeetingAttendees Operation` parameter value is invalid | The `Operation` value must be Mute or Unmute\. | 
|   | Meeting ID parameter value is invalid\. | Meeting ID is empty\. | 
|   | Attendee List parameter value is invalid\. | The Attendee ID list is empty\. | 
|   | Invalid action on the call\. | The call isn't bridged\. | 
|   | Call is not connected to Chime Meeting\. | The attendee is not connected to a Chime Meeting\. | 
|   | One or more attendees are not part of this meeting\. All attendees must be part of this meeting\. | The attendee is not authorized to modify attendees in the meeting\. | 
| `SystemException` | System error while running action\. | A system error occurred while running an action\. | 

The following example code shows a typical failure event:

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type" : "ModifyChimeMeetingAttendees",
        "Parameters" : {
            "Operation": "Mute", //or Unmute
            "MeetingId": "meeting-id",
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