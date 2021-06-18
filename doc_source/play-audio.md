# PlayAudio<a name="play-audio"></a>

Play an audio file on any leg of a call\. For example, you can play a message to the caller during an Interactive Voice Response \(IVR\) call flow\. You must use audio files in an Amazon S3 bucket\. The S3 bucket must belong to the same AWS account as the SIP media application\. In addition, you must give the `s3:GetObject` permission to the [Amazon Chime Voice Connector service principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)—`voiceconnector.chime.amazonaws.com`\.

The following example shows a typical S3 bucket policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SMARead",
            "Effect": "Allow",
            "Principal": {
                "Service": "voiceconnector.chime.amazonaws.com"
            },
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::bucket-name/*"
        }
    ]
}
```

The following example shows a typical action\.

```
{
    "Type" : "PlayAudio",    
    "Parameters" : {
        "CallId": "call-id-1",
        "ParticipantTag": "LEG-A",
        "AudioSource": {
           "Type": "S3",
           "BucketName": "bucket-name",
           "Key": "wave-file.wav"
         }
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

**AudioSource\.Type**  
*Description* – Type of source for audio file  
*Allowed values* – S3  
*Required* – Yes  
*Default value* – None

**AudioSource\.BucketName**  
*Description* – For *S3\-bucket\-name* values, the S3 bucket must belong to the same AWS account as the SIP application\. The [Amazon Chime Voice Connector service principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)—`voiceconnector.chime.amazonaws.com`, must have access to the S3 bucket\.\.  
*Allowed values* – A valid S3 bucket for which Amazon Chime has access to the `s3:GetObject` action\.  
*Required* – Yes  
*Default value* – None

**AudioSource\.key**  
*Description* – The key name of the audio object in the `AudioSource.BucketName` S3 bucket\.  
*Allowed values* – A valid audio file  
*Required* – Yes  
*Default value* – None

The SIP media application tries to play the audio from the source URL\. You must use PCM WAV files of no more than 50 MB in size\. &CHM; recommends using 8 KHz mono\.

The S3 metadata for each WAV file must contain `{'ContentType': 'audio/wav'}`\.

When the last instruction in a dialplan is `PlayAudio` and the file finishes playback, or if a user stops playback with a key press, the application invokes the Lambda function with the event shown in the following example\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "PlayAudio",
        "Parameters" : {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "wave-file.wav",
         }           
     }
}
```

After a terminating digit stops the audio, it won't be repeated\.

**Error handling**  
If an error occurs while executing an action, the SIP media application calls the Lambda function with the appropriate error code\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidAudioSource`  |  The audio source parameter is invalid\.  |  This error can happen for multiple reasons\. For example, the SIP media application cannot access the file due to permission issues, or issues with the URL\. Or, the audio file may fail validation due to format, duration, size, and so on\.  | 
|  `SystemException`  |  System error while executing action\.  |  Another system error occurred while executing the action\.   | 
|  `InvalidActionParameter`  |  `CallId` or `ParticipantTag` parameter for action is invalid\.  |  The action contains an invalid parameter\.  | 

The following example shows a typical invocation failure\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 2,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type": "PlayAudio",
        "Parameters" : {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "audio-file.wav"
            },
        },
        "ErrorType": "InvalidAudioSource",
        "ErrorMessage": "Audio Source parameter value is invalid."
    }
    "CallDetails": {
        ...
    }
}
```