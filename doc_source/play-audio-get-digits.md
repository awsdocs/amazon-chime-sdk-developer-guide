# PlayAudioAndGetDigits<a name="play-audio-get-digits"></a>

Plays audio and gathers DTMF digits\. If a failure occurs, such as a user not entering the correct number of DTMF digits, the action plays the "failure" audio and then replays the main audio until the SIP media application exhausts the number of attempts defined in the `Repeat` parameter\.

You must play audio files from the S3 bucket\. The S3 bucket must belong to the same AWS account as the SIP media application\. In addition, you must give the `s3:GetObject` permission to the [ Amazon Chime Voice Connector service principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html), `voiceconnector.chime.amazonaws.com`\. You can use the S3 console or the CLI to do that\. 

The following example shows an S3 bucket policy\.

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

The following example shows a typical `PlayAudioAndGetDigits` action\. 

```
{
    "Type" : "PlayAudioAndGetDigits",
    "Parameters" : {
        "CallId": "call-id-1",
        "ParticipantTag": "LEG-A"      
        "InputDigitsRegex": "^\d{2}#$",
        "AudioSource": {
            "Type": "S3",
            "BucketName": "bucket-name",
            "Key": "audio-file-1.wav"
        },
        "FailureAudioSource": {
            "Type": "S3",
            "BucketName": "bucket-name",
            "Key": "audio-file-failure.wav"
        },
        "MinNumberOfDigits": 3,
        "MaxNumberOfDigits": 5,
        "TerminatorDigits": ["#"],        
        "InBetweenDigitsDurationInMilliseconds": 5000,
        "Repeat": 3,
        "RepeatDurationInMilliseconds": 10000
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

**InputDigitsRegex**  
*Description* – A regular expression pattern  
*Allowed values* – A valid regular expression pattern  
*Required* – No  
*Default value* – None

**AudioSource\.Type**  
*Description* – Type of source for the audio file type  
*Allowed values* – An S3 bucket  
*Required* – Yes  
*Default value* – `"S3"`

**AudioSource\.BucketName**  
*Description* – For S3 `AudioSource.Type` values, the S3 bucket must belong to the same AWS account as the SIP media application\. The bucket S3 must have access to the [Amazon Chime Voice Connector service principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html), `voiceconnector.chime.amazonaws.com`\.  
*Allowed values* – A valid S3 bucket for which Amazon Chime has `s3:GetObject` actions access\.  
*Required* – Yes  
*Default value* – None

**AudioSource\.Key**  
*Description* – The key name of the audio object in the `AudioSource.BucketName` S3 bucket\.  
*Allowed values* – Valid audio files  
*Required* – Yes  
*Default value* – None

**FailureAudioSource\.Type**  
*Description* – The key name of the audio object in the `FailureAudioSource.BucketName` S3 bucket\.  
*Allowed values* – S3  
*Required* – Yes  
*Default value* – None

**FailureAudioSource\.BucketName**  
*Description* – For S3 source types, the S3 bucket must belong to the same AWS account as the SIP media application\. The [Amazon Chime Voice Connector service principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html), `voiceconnector.chime.amazonaws.com`, must have access to the S3 bucket\.  
*Allowed values* – A valid S3 bucket for which Amazon Chime has `s3:GetObject` actions access\.  
*Required* – Yes  
*Default value* – None

**FailureAudioSource\.Key**  
*Description* – The key name of the audio object in the `FailureAudioSource.BucketName` S3 bucket\.  
*Allowed values* – Valid audio files  
*Required* – Yes  
*Default value* – None

**MinNumberOfDigits**  
*Description* – The minimum number of digits to capture before timing out or playing "call failed" audio\.  
*Allowed values* – >=0  
*Required* – No  
*Default value* – 0

**MaxNumberOfDigits**  
*Description* – The maximum number of digits to capture before stopping without a terminating digit\.  
*Allowed values* – >`MinNumberOfDigits`  
*Required* – No  
*Default value* – 128

**TerminatorDigits**  
*Description* – Digits used to end input if the user enters less than the `MaxNumberOfDigits`  
*Allowed values* – Any one of these digits: 0123456789\#\*  
*Required* – No  
*Default value* – \#

**InBetweenDigitsDurationInMilliseconds**  
*Description* – The wait time in milliseconds between digit inputs before playing `FailureAudio`\.  
*Allowed values* – >0  
*Required* – No  
*Default value* – If not specified, defaults to the `RepeatDurationInMilliseconds` value\.

**Repeat**  
*Description* – Total number of attempts to get digits  
*Allowed values* – >0  
*Required* – No  
*Default value* – 1

**RepeatDurationInMilliseconds**  
*Description* – Time in milliseconds to wait between `Repeat` attempts  
*Allowed values* – >0  
*Required* – Yes  
*Default value* – None

The SIP media application always invokes its Lambda function after running the `PlayAudioAndGetDigits` action, with an `ACTION_SUCCESSFUL` or `ACTION_FAILED` invocation event type\. When the application successfully gathers digits, it sets the `ReceivedDigits` value in the `ActionData` object\. The following example shows the invocation event structure of that Lambda function\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "PlayAudioAndGetDigits",
        "Parameters" : {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            "InputDigitsRegex": "^\d{2}#$",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "audio-file-1.wav"
            },
            "FailureAudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "audio-file-failure.wav"
            },
            "MinNumberOfDigits": 3,
            "MaxNumberOfDigits": 5,
            "TerminatorDigits": ["#"],
            "InBetweenDigitsDurationInMilliseconds": 5000,
            "Repeat": 3,
            "RepeatDurationInMilliseconds": 10000
        },
        "ErrorType": "InvalidAudioSource",
        "ErrorMessage": "Audio Source parameter value is invalid."
    },
        "ReceivedDigits": "1234"
    },
    "CallDetails": {
        ...
    }
}
```

**Error handling**  
When a validation error occurs, the SIP media application calls the Lambda function with the corresponding error message\. The following table lists the possible error messages\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidAudioSource`  |  Audio source parameter value is invalid\.  |  This error can happen for multiple reasons\. For example, the SIP media application cannot access the file due to permission issues, or issues with the S3 bucket\. Or, the audio file may fail validation due to duration, size, or unsupported format\.  | 
|  `InvalidActionParameter`  |  `CallId` or `ParticipantTag` parameter for the action is invalid\.  |  A `CallId`, `ParticipantTag`, or other parameter is not valid\.  | 
|  `SystemException`  |  System error while executing the action\.  |  A system error occurred while executing the action\.  | 

When the action fails to collect the number of specified digits due to a timeout or too many retries, the SIP media application invokes the Lambda function with the `ACTION_FAILED` invocation event type\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 4,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type": "PlayAudioAndGetDigits",
        "Parameters" : {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            "InputDigitsRegex": "^\d{2}#$",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "audio-file-1.wav"
            },
            "FailureAudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "audio-file-failure.wav"
            },
            "MinNumberOfDigits": 3,
            "MaxNumberOfDigits": 5,
            "TerminatorDigits": ["#"],
            "InBetweenDigitsDurationInMilliseconds": 5000,
            "Repeat": 3,
            "RepeatDurationInMilliseconds": 10000
        },
        "ErrorType": "InvalidAudioSource",
        "ErrorMessage": "Audio Source parameter value is invalid."
    }
    "CallDetails": {
        ...
    }
}
```