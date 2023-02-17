# Understanding the PSTN Audio service programming model<a name="pstn-model"></a>

The PSTN Audio service uses a request/response programming model that in turn uses AWS Lambda functions\. Your AWS Lambda function is invoked automatically for incoming and outgoing calls\. For example, when a new incoming call arrives, the PSTN Audio service invokes your AWS Lambda function with a `NEW_INCOMING_CALL` event and waits for commands called *Actions*\. For example, your application can choose actions such as playing an audio prompt, collecting digits, recording audio, or routing the call onward\. These JSON formatted actions are sent back to the PSTN Audio service using a callback from your AWS Lambda function\. 

This example shows a `PlayAudio` action\.

```
{
    "Type": "PlayAudio",
    "Parameters": {
        "CallId": "call-id-1",
        "ParticipantTag": "LEG-A",
        "PlaybackTerminators": ["1", "8", "#"],
        "Repeat": "5",
        "AudioSource": {
            "Type": "S3",
            "BucketName": "valid-S3-bucket-name",
            "Key": "wave-file.wav"
        }
    }
}
```

This example shows a `RecordAudio` action\.

```
{
    "Type": "RecordAudio",
    "Parameters": {
        "CallId": "call-id-1",
        "DurationInSeconds": "10",
        "SilenceDurationInSeconds": 3,
        "SilenceThreshold": 100,
        "RecordingTerminators": [
            "#"
        ],
        "RecordingDestination": {
            "Type": "S3",
            "BucketName": "valid-bucket-name",
            "Prefix": "valid-prefix-name"
        }
    }
}
```

Once the PSTN Audio service runs the action, it invokes your AWS Lambda function again with either a success or failure indication\. 

Your application can also make outbound phone calls and use your AWS Lambda function to control the call flow, caller experience, and call context\. In this case, you call the [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateSipMediaApplicationCall.html) API, and your AWS Lambda is invoked with a `NEW_OUTBOUND_CALL` event\. Once the call is answered, you can return actions, such as playing a voice prompt and collecting user\-entered digits\. You can also trigger your AWS Lambda function using the [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API to implement timers, participant muting, and waiting rooms\.