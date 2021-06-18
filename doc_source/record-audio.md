# RecordAudio<a name="record-audio"></a>

Allows the SIP media application to record media from a given call ID\. For example, a voice mail application and meeting\-participant announcements\. The application records until it reaches the duration that you set, or when a user presses one of the `RecordingTerminators`\. In those cases, the action tells your application to put the resulting media file into the specified S3 bucket\. The S3 bucket must belong to the same AWS account as the SIP media application\. In addition, the action must give `s3:PutObject` and `s3:PutObjectAcl` permission to the Amazon Chime Voice Connector service principal, [ Amazon Chime Voice Connector service principal ](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html), `voiceconnector.chime.amazonaws.com`\. 

Note that recordings made using this feature may be subject to laws or regulations regarding the recording of electronic communications\. It is your and your end users’ responsibility to comply with all applicable laws regarding the recording, including properly notifying all participants in a recorded session or communication that the session or communication is being recorded, and obtaining their consent\.

The following example gives the `s3:PutObject` and `s3:PutObjectAcl` permission to the Amazon Chime Voice Connector service principal\.

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
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::bucket-name/"
        }
    ]
}
```

The following example stops recording when the caller presses the pound key \(\#\) or 10 seconds elapse with no activity\. The `RecordinDestination` parameter sends the resulting media file to the specifed S3 bucket\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Type" : "RecordAudio",
            "Parameters" : {
                "CallId": "call-id-1",
                "ParticipantTag": "LEG-A",
                "DurationInSeconds": "10",
                "RecordingTerminators": ["#"],
                "RecordinDestination": {
                "Type": "S3",
                "BucketName": "valid-bucket-name"
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

**RecordingDestination\.Type**  
*Description* – Type of destination\. Only S3\.  
*Allowed values* – S3  
*Required* – Yes  
*Default value* – None

**RecordingDestination\.BucketName**  
*Description* – A valid S3 bucket name\. The bucket must have access to the [Amazon Chime Voice Connector service principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html), `voiceconnector.chime.amazonaws.com`\.  
*Allowed values* – A valid S3 bucket for which Amazon Chime has access to the `s3:PutObject` and `s3:PutObjectAcl` actions\.  
*Required* – Yes  
*Default value* – None

**DurationInSeconds**  
*Description* – The duration of the recording, in seconds  
*Allowed values* – >0  
*Required* – Yes  
*Default value* – None

**RecordingTerminators**  
*Description* – Stops the recording when the user presses any of the digits associated with the attribute\.  
*Allowed values* – An array of single digits and symbols from \[123456789\*0\#\]  
*Required* – Yes  
*Default value* – None

When the recording ends, the SIP media application calls the Lambda function, complete with a file name and S3 bucket path\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type" : "RecordAudio",
        "Parameters" : {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            "DurationInSeconds": "10",
            "RecordingTerminators": ["#"],
            "RecordingDestination": {
                "Type": "S3",
                "BucketName": "valid-bucket-name"
            }
        },
        "RecordinDestination": {
            "Type": "S3",
            "BucketName": "valid-bucket-name",
            "Key": "call-id-1-recordings-counter.wav"
        }
    }
    "CallDetails": {
        ...
    }
}
```

**Error handling**  
For validation errors, the SIP media application calls the Lambda function with the appropriate error message\. The following table lists the possible error messages\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidActionParameter`  |  `CallId` or `ParticipantTag` parameter for action is invalid  |  Any parameter is invalid\.  | 
|  `SystemException`  |  System error while executing an action\.  |  Another type of system error occurred while executing an action\.  | 

When the action fails to record the media on a call leg, the SIP media application invokes a Lambda function with the `ActionFailed` event type\. See the following example\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 5,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type" : "RecordAudio",
        "Parameters" : {
            "ParticipantTag": "LEG-A",
            "DestinationType": "S3",
            "DestinationValue": "https://doc-example-bucket.s3.us-west-2.amazonaws.com/error-message.wav",
            "MaxDurationInSeconds": "10",
            "StopRecordingOnDigits": ["#"]
        },
        "Error": "NoAccessTODestination: Error while accessing destination"
    }
    "CallDetails": {
        ...
    }
}
```