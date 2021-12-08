# PauseCallRecording<a name="pause-call-recording"></a>

The `PauseCallRecording` action pauses recording of a call leg\. Each time you pause a recording, the recording captures a tone that indicates the pause\. When you pause, the recording continues, but silence is captured\. Pausing the recording does not affect the total duration of the recording\. You can pause and resume recording as often as needed\.

The following example pauses recording\. 

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "PauseCallRecording",
            "Parameters": {
                "CallId": "call-id-1"
            }
        }
    ]
}
```

**CallId**  
*Description* – `CallId` of participant in the `CallDetails` of the AWS Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – Yes  
*Default value* – None