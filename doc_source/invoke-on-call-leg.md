# Responding to invocations with action lists<a name="invoke-on-call-leg"></a>

You can respond to a Lambda invocation event with a list of actions to run on the individual participants in a call\. The following example shows the general response structure\.

```
{
    "SchemaVersion": "1.0",
    "Actions": [        
        {
            "Type": "PlayAudio",
            "Parameters": {
                "ParticipantTag": "LEG-A",
                "AudioSource": {
                    "Type": "S3",
                    "BucketName": "bucket-name",
                    "Key": "audio-file.wav"
                }
            }
        },
        {
            "Type": "RecordAudio",
            "Parameters": {
                "DurationInSeconds": "10",
                "RecordingTerminators": ["#"],
                "RecordingDestination": {
                    "Type": "S3",
                    "BucketName": "bucket-name"
                }
            }
        }
    ]
}
```

When the Lambda function returns the list of action to the SIP media application, the following operations occur:

1. The application finishes running the current action on a call\.

1. The application then replaces the old action set with a new set of actions received from the latest invocation event\.

If the SIP media application receives a NULL action set, it keeps the existing actions\. 