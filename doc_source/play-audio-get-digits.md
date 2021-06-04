# PlayAudioAndGetDigits<a name="play-audio-get-digits"></a>

Plays audio and gathers dual\-tone multi frequency \(DTMF\) digits\. If a failure occurs, such as a user not entering the correct number of digits, the action plays the "failure" audio and then replays the main audio\.

You can play audio files from the Amazon Simple Storage Service \(Amazon S3\) bucket\. The bucket must belong to the same AWS account as the SIP media application\. In addition, you must give the `s3:GetObject` permission to the Amazon Chime Voice Connector service principal\. You can do that by using the S3 console or the command\-line interface \(CLI\)\.

The following code example shows an S3 bucket policy\.

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

The following code example shows a typical `PlayAudioAndGetDigits` action\. 

```
{
    "Type" : "PlayAudioAndGetDigits",
    "Parameters" : {
        "CallId": "call-id-1",
        "ParticipantTag": "LEG-A", //or LEG-B        
        "InputDigitsRegex": "^\d{2}#$",
        "AudioSource": {
            "Type": "S3",
            "BucketName": "valid-s3-bucket-name",
            "Key": "audio-file-1.wav"
        },
        "FailureAudioSource": {
            "Type": "S3",
            "BucketName": "valid-s3-bucket-name",
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

**InputDigitsRegex**  
*Description* – A regex pattern\.  
*Allowed values* – A valid regular expression pattern\.  
*Required* – No\.  
*Default value* – None\.

**AudioSource\.Type**  
*Description* – Type of source for the audio file type\.  
*Allowed values* – A remote URL\.  
*Required* – Yes\.  
*Default value* – `RemoteURL`\.

**AudioSource\.BucketName**  
*Description* – For S3 `AudioSource.Type` values, the S3 bucket must belong to the same AWS account as the SIP media application\. The bucket must have access to the Amazon Chime Voice Connector service principal, which is voiceconnector\.chime\.amazonaws\.com\.  
*Allowed values* – A valid S3 bucket for which Amazon Chime has `s3:GetObject` actions access\.  
*Required* – Yes\.  
*Default value* – None\.

**AudioSource\.Key**  
*Description* – For S3 source types, the file name from the S3 bucket specified in `AudioSource.BucketName`\.  
*Allowed values* – Valid audio files\.  
*Required* – Yes\.  
*Default value* – None\.

**FailureAudioSource\.Type**  
*Description* – Type of source for failure audio file\.  
*Allowed values* – S3\.  
*Required* – Yes\.  
*Default value* – None\.

**FailureAudioSource\.BucketName**  
*Description* – For S3 source types, the S3 bucket must belong to the same AWS account as the SIP media application\. The bucket should have access to the Amazon Chime Voice Connector service principal, which is voiceconnector\.chime\.amazonaws\.com\.  
*Allowed values* – A valid S3 bucket for which Amazon Chime has `s3:GetObject` actions access\.  
*Required* – Yes\.  
*Default value* – None\.

**FailureAudioSource\.Key**  
*Description* – For S3 types, the file name from the S3 bucket specified in the `AudioSource.BucketName` attribute\.  
*Allowed values* – Valid audio files\.  
*Required* – Yes\.  
*Default value* – None\.

**MinNumberOfDigits**  
*Description* – The minimum number of digits to get before timing out or playing a "call failed" audio clip\.  
*Allowed values* – >=0\.  
*Required* – No\.  
*Default value* – 0\.

**MaxNumberOfDigits**  
*Description* – The maximum number of digits to get before stopping without a terminating digit\.  
*Allowed values* – >`MinNumberOfDigits`\.  
*Required* – No\.  
*Default value* – 128\.

**TerminatorDigits**  
*Description* – Digits used to end input if the user enters less than the `MaxNumberOfDigits`\.  
*Allowed values* – Any one of these digits: 0123456789\#\*  
*Required* – No\.  
*Default value* – \#\.

**InBetweenDigitsDurationInMilliseconds**  
*Description* – The wait time in milliseconds between digit inputs before playing `FailureAudio`\.  
*Allowed values* – >0\.  
*Required* – No\.  
*Default value* – If not specified, the `RepeatDurationInMilliseconds` value\.

**Repeat**  
*Description* – Total number of attempts to get digits\.  
*Allowed values* – >0\.  
*Required* – No\.  
*Default value* – 1\.

**RepeatDurationInMilliseconds**  
*Description* – Time in milliseconds to wait before next attempt\.  
*Allowed values* – >0\.  
*Required* – Yes\.  
*Default value* – None\.

Your SIP media application always invokes its Lambda function after running this action, with an `ACTION_SUCCESSFUL` or `ACTION_FAILED` event type\. When the application successfully gathers digits, it sets the `ReceivedDigits` variable in the `ActionData` object\. The following code example shows the event structure of that Lambda function\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "PlayAudioAndGetDigits",
        "Parameters" : {
            "ParticipantTag": "LEG-A",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "valid-S3-bucket-name",
                "Key": "audio-file.mp3"
            },
            "FailureAudioSource": {
                "Type": "S3",
                "BucketName": "valid-S3-bucket-name",
                "Key": "failure-audio-file.wav"
            },
            "MinNumberOfDigits": 3,
            "MaxNumberOfDigits": 5,
            "TerminatorDigits": ["#"],
            "InBetweenDigitsDurationInMilliseconds": 5000,
            "Repeat": 3,
            "RepeatDurationInMilliseconds": 10000
        },
        "ReceivedDigits": "1234"
    },
    "CallDetails": {
        ...
    }
}
```

**Error handling**  
When a validation error occurs, the SIP media application calls the Lambda function with the appropriate error message\. The following table lists the error messages\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidAudioSource`  |  Audio source parameter value is invalid\.  |  This error can happen for multiple reasons\. For example, the SIP media applicattion cannot access the file due to permission issues, or issues with the URL\. Or, the audio file may fail validation due to format, duration, size, and so on\.  | 
|  `InvalidActionParameter`  |  CallId or ParticipantTag parameter for action is invalid\.  |  A `CallId`, `ParticipantTag`, or other parameter is not valid\.  | 
|  `SystemException`  |  System error while running action\.  |  A system error occurred while running the action\.  | 

When the action fails to collect the number of specified digits due to a timeout or too many retries, the SIP media application invokes a Lambda function with the `ActionFailed` event type\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 4,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type": "PlayAudioAndGetDigits",
        "Parameters" : {
            "ParticipantTag": "LEG-A",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "valid-S3-bucket-name",
                "Key": "audio-file.mp3"
            },
            "FailureAudioSource": {
                "Type": "S3",
                "BucketName": "valid-S3-bucket-name",
                "Key": "failure-audio.mp3"
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