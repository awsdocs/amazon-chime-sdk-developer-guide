# ResumeCallRecording<a name="resume-call-recording"></a>

The `ResumeCallRecording` action resumes the recording of a call leg\. Before the recording restarts, a brief tone is played\. You can pause and resume a recording multiple times for the duration of the call leg\. 

The following example resumes recording\. 

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "ResumeCallRecording",
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

See a working example on GitHub: [https://github\.com/aws\-samples/amazon\-chime\-sma\-on\-demand\-recording](https://github.com/aws-samples/amazon-chime-sma-on-demand-recording)