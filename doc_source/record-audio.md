# RecordAudio<a name="record-audio"></a>

Allows your SIP application to record media from a given call ID\. The application records until it reaches the duration that you set, or when a user presses one of the `RecordingTerminators`\. In those cases, the action tells your application to send the media file to the specified Amazon Simple Storage Service \(Amazon S3\) bucket\. The S3 bucket must belong to the same AWS account as the SIP application\. In addition, the action must give `s3:PutObject` and `s3:PutObjectAcl` permission to the Amazon Chime Voice Connector service principal\. 

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
                "s3:GetObject",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::bucket-name/"
        }
    ]
   }
{
    "Type" : "RecordAudio",
    "Parameters" : {
        "CallId": "call-id-1",
        "DurationInSeconds": "10",
        "RecordingTerminators": ["#"],
        "RecordinDestination": {
            "Type": "S3",
            "BucketName": "valid-bucket-name"
        }
    }
}
```

**CallID**  
*Description* – `CallId` of participant in the `CallDetails`  
*Allowed values* – A valid call ID  
*Required* – Yes  
*Default value* – None

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`  
*Allowed values* – `LEG-A` or `LEG-B`  
*Required* – Yes  
*Default value* – `ParticipantTag` of the invoked `callLeg`\. Ignored if you specify `CallId`\.

**RecordingDestination\.Type**  
*Description* – Type of destination\. Only S3\.  
*Allowed values* – S3  
*Required* – Yes  
*Default value* – None

**RecordingDestination\.BucketName**  
*Description* – For S3 recording destinations, you must provide a valid S3 bucket name\. The bucket must have access to the Amazon Chime Voice Connector service principal, which is voiceconnector\.chime\.amazonaws\.com\.  
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

When the recording ends, the application calls its Lambda function, complete with a file name and S3 bucket path\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type" : "RecordAudio",
        "Parameters" : {
            "CallId": "call-id-1",
            "DurationInSeconds": "10",
            "RecordingTerminators": ["#"],
            "RecordinDestination": {
                "Type": "S3",
                "BucketName": "valid-bucket-name"
            }
        },
        "RecordinDestination": {
            "Type": "S3",
            "BucketName": "valid-bucket-name",
            "Key": "call-id-1-recordings-counter.mp3"
        }
    }
    "CallDetails": {
        ...
    }
}
```

**Error handling**  
For validation errors, the application calls the Lambda function with the appropriate error message\. The following table lists the error messages\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidActionParameter`  |  CallId or ParticipantTag parameter for action is invalid  |  `callId` or other parameter is invalid\.  | 
|  `SystemException`  |  System error while running an action\.  |  Another type of system error occurred while running an action\.  | 

When the action fails to record the media on a call leg, the SIP application invokes a Lambda function with the `ActionFailed` event type\. See the following code example\.

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
            "DestinationValue": "/path/to/s3/bucket",
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