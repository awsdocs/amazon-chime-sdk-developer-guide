# StopCallRecording<a name="stop-call-recording"></a>

The `StopCallRecording` action stops the recording of a call leg\. Call recordings automatically stop when a call ends, and in that case, you your application doesn't need to explicitly return the `StopCallRecording` action\. Once recording for a call leg stops, it can’t start again, and the recording is delivered to the destination specified in the `StartCallRecording` action\. 

The following example stops recording for the `call-id-1` call leg\. 

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "StopCallRecording",
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