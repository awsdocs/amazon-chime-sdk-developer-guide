# Pause<a name="pause"></a>

Pause a call for a specified time\.

```
{
    "Type": "Pause",
    "Parameters": {
        "CallId": "call-id-1",
        "ParticipantTag": "LEG-A",
        "DurationInMilliseconds": "3000"
    }
}
```

**CallId**  
*Description* – `CallId` of participant in the `CallDetails` of the Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – No  
*Default value* – None

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`  
*Allowed values* – `LEG-A` or `LEG-B`  
*Required* – No  
*Default value* – `ParticipantTag` of the invoked `callLeg` Ignored if you specify `CallId`

**DurationInMilliseconds**  
*Description* – Duration of the pause, in milliseconds  
*Allowed values* – An integer >0  
*Required* – Yes  
*Default value* – None