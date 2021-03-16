# Responding to invocations with action lists<a name="invoke-on-call-leg"></a>

You can respond to a Lambda invocation event with a list of actions to run on the individual participants in a call\. The following code example shows the general response structure\.

```
{
    "SchemaVersion": "1.0",
    "Actions": [
        // List of supported Actions
        {
            "Type": "PlayAudio",
            "Parameters": {
                "ParticipantTag": "LEG-A",
                "AudioSource": {
                    "Type": "S3",
                    "BucketName": "valid-S3-bucket-name",
                    "Key": "audio-file.wav"
                }
            }
        },
        ...
    ]
}
```

Whenever a SIP application invokes a Lambda function, the following operations occur:

1. The application finishes running the current action on a call\.

1. The application then replaces the old action set with a new set of actions received from the latest invocation event\.

1. If the SIP application doesn't receive new actions from the Lambda function, it keeps the existing actions\. 