# PlayAudio<a name="play-audio"></a>

Play an audio file on any leg of a call\. The audio can be repeated any number of times\. The in\-progress audio can be terminated using the DTMF digits set in the `PlaybackTerminators`\.

Currently, Amazon Chime SDK only supports playing audio files from the Amazon Simple Storage Service \(Amazon S3\) bucket\. The S3 bucket must belong to the same AWS account as the SIP media application\. In addition, you must give the `s3:GetObject` permission to the Amazon Chime SDK Voice Connector service principal\. You can do that by using the S3 console or the command\-line interface \(CLI\)\.

The following code example shows a typical bucket policy\.

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
            "Resource": "arn:aws:s3:::bucket-name/*",
                "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "aws-account-id"
                }
            }
        }
    ]
}
```

The PSTN Audio service reads and writes to your S3 bucket on behalf of your Sip Media Application\. To avoid the [confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html), you can restrict S3 bucket access to a single SIP media application\.

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
            "Resource": "arn:aws:s3:::bucket-name/*",
                "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "aws-account-id",
                    "aws:SourceArn": "arn:aws:chime:region:aws-account-id:sma/sip-media-application-id"
                }
            }
        }
    ]
}
```

The following code example shows a typical action\.

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

**CallID**  
*Description* – `CallId` of participant in the `CallDetails`\.  
*Allowed values* – A valid call ID\.  
*Required* – No, if `ParticipantTag` is present\.  
*Default value* – None\.

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`\.  
*Allowed values* – `LEG-A` or `LEG-B`\.  
*Required* – No, if `CallId` is present\.  
*Default value* – `ParticipantTag` of the invoked `callLeg`\. Ignored if you specify `CallId`\.

**PlaybackTerminator**  
*Description* – Terminates in\-progress audio by using DTMF input from the user  
*Allowed values* – An array of the following values; “0”, ”1”, ”2”, ”3”, ”4”, ”5”, ”6”, ”7”, ”8”, ”9”, ”\#”, ”\*”  
*Required* – No  
*Default value* – None

**Repeat**  
*Description* – Repeats the audio the specified number of times  
*Allowed values* – An integer greater than zero  
*Required* – No  
*Default value* – 1

**AudioSource\.Type**  
*Description* – Type of source for audio file\.  
*Allowed values* – S3\.  
*Required* – Yes\.  
*Default value* – None\.

**AudioSource\.BucketName**  
*Description* – For S3 source types, the S3 bucket must belong to the same AWS account as the SIP application\. The bucket must have access to the Amazon Chime SDK Voice Connector service principal, which is voiceconnector\.chime\.amazonaws\.com\.  
*Allowed values* – A valid S3 bucket for which Amazon Chime SDK has access to the `s3:GetObject` action\.  
*Required* – Yes\.  
*Default value* – None\.

**AudioSource\.key**  
*Description* – For S3 source types, the file name from the S3 bucket specified in the `AudioSource.BucketName` attribute\.  
*Allowed values* – A valid audio file\.  
*Required* – Yes\.  
*Default value* – None\.

The SIP media application tries to play the audio from the source URL\. You can use raw, uncompressed PCM \.wav files of no more than 50 MB in size\. Amazon Chime SDK recommends 8 KHz mono\.

When the last instruction in a dialplan is `PlayAudio` and the file finishes playback, or if a user stops playback with a key press, the application invokes the AWS Lambda function with the event shown in the following code example\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "PlayAudio",
        "Parameters" : {
            "CallId": "call-id-1",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "valid-S3-bucket-name",
                "Key": "wave-file.wav",
         }           
     }
}
```

After a terminating digit stops the audio, it won't be repeated\.

**Error handling**  
When the validation file contains errors, or an error occurs while running an action, the SIP media application calls an AWS Lambda function with the appropriate error code\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidAudioSource`  |  The audio source parameter is invalid\.  |  This error can happen for multiple reasons\. For example, the SIP media application cannot access the file due to permission issues, or issues with the URL\. Or, the audio file may fail validation due to format, duration, size, and so on\.  | 
|  `SystemException`  |  System error while running action\.  |  Another system error occurred while running the action\.   | 
|  `InvalidActionParameter`  |  CallId or ParticipantTag parameter for action is invalid\.  |  The action contains an invalid parameter\.  | 

The following code example shows a typical invocation failure\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 2,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type": "PlayAudio",
        "Parameters" : {
            "CallId": "call-id-1",
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

See working examples on GitHub:
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-bridging](https://github.com/aws-samples/amazon-chime-sma-bridging)\.
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-call\-forwarding](https://github.com/aws-samples/amazon-chime-sma-call-forwarding)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-outbound\-call\-notifications](https://github.com/aws-samples/amazon-chime-sma-outbound-call-notifications)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-on\-demand\-recording](https://github.com/aws-samples/amazon-chime-sma-on-demand-recording)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-update\-call](https://github.com/aws-samples/amazon-chime-sma-update-call)