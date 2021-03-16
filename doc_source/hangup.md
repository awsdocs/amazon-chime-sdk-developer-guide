# Hangup<a name="hangup"></a>

Sends a `Hangup` value with a `SipStatusCode` to any leg of a call\. See the following code example\.

```
{
    "Type": "Hangup",
    "Parameters": {
        "ParticipantTag": "MeetingParticipant1",
        // 480 - Unavailable, 486 - Busy, 0 - Terminated
        "SipResponseCode": "486"
    }
}
```

**CallID**  
*Description* – `CallId` of participant in the `CallDetails`  
*Allowed values* – A valid call ID  
*Required* – Yes  
*Default value* – None

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`  
*Allowed values* – `LEG-A` or `LEG-B`  
*Required* – Yes  
*Default value* – `ParticipantTag` of the invoked `callLeg`\. Ignored if you specify `CallId`\.

**SipResponseCode**  
*Description* – Any of the supported SIP response codes\.  
*Allowed values* – 480–Unavailable; 486–Busy; 0–Normal Termination  
*Required* – No  
*Default value* – 0

After a user ends a call, the SIP application invokes a Lambda function with the code listed in [Ending a call](case-5.md)\.