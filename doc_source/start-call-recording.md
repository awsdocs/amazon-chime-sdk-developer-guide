# StartCallRecording<a name="start-call-recording"></a>

The `StartCallRecording` action starts the recording of a call leg\. You start call recording in your SIP media applications, either on\-demand or in response to a SIP event\.
+ To start on\-demand recording of a call, you use the `UpdateSipMediaApplication` API to invoke your application and return the `StartCallRecording` action\.
+ To start call recording in response to a SIP event, you return the `StartCallRecording` action in your application\. 

You specify whether you want to record the audio track for the incoming track, the outgoing track, or both tracks of the call leg\. The following sections explain how to use the `StartCallRecording` action\.

**Note**  
Recordings made using this feature may be subject to laws or regulations regarding the recording of electronic communications\. It is your and your end users’ responsibility to comply with all applicable laws regarding the recording, including properly notifying all participants in a recorded session or communication that the session or communication is being recorded, and obtaining their consent\.

**Topics**
+ [Requesting a StartCallRecording action](#request-start)
+ [Specifying a recording destination](#recording-destination)
+ [Granting Amazon S3 bucket permissions](#grant-s3-perms)
+ [Action successful response](#action-successful)
+ [Action error response](#action-error)

## Requesting a StartCallRecording action<a name="request-start"></a>

The following example shows how to request the `StartCallRecording` action for `BOTH` tracks\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Type": "StartCallRecording",
            "Parameters":
            {
                "CallId": "call-id-1",
                "Track": "BOTH",
                "Destination":
                {
                    "Type": "S3",
                    "Location": "valid-bucket-name-and-optional-prefix"
                }
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

**Track**  
*Description* – Audio `Track` of the call recording\.  
*Allowed values* – `BOTH`, `INCOMING`, or `OUTGOING`  
*Required* – Yes  
*Default value* – None

**Destination\.Type**  
*Description* – Type of destination\. Only Amazon S3 is allowed\.  
*Allowed values* – Amazon S3  
*Required* – Yes  
*Default value* – None

**Destination\.Location**  
*Description* – A valid Amazon S3 bucket and an optional Amazon S3 key prefix\. The bucket must have access to the Amazon Chime Voice Connector service principal, voiceconnector\.chime\.amazonaws\.com\.  
*Allowed values* – A valid Amazon S3 path for which Amazon Chime has access to the `s3:PutObject` and `s3:PutObjectAcl` actions\.  
*Required* – Yes  
*Default value* – None

## Specifying a recording destination<a name="recording-destination"></a>

Amazon Chime delivers call recordings to your Amazon S3 bucket\. The bucket must belong to your AWS account\. You specify the bucket's location in the `Destination` parameter of the `StartCallRecording` action\. The `Type` field in the the `Destination` parameter must be `S3`\. The `Location` field consists of your Amazon S3 bucket, plus an optional object\-key prefix in which the call recording is delivered\. 

The SIP media application uses the specified `Location`, the call leg’s date and time, the transaction ID, and the call ID to format the Amazon S3 object key\. The `StartCallRecording` action response returns the full Amazon S3 object key\.

When you only provide the Amazon S3 bucket in the `Location` field, the SIP media application appends a default prefix, `Amazon-Chime-SMA-Call-Recordings`, to the Amazon S3 path\. The SIP media application also appends the year, month, and day of the call to help organize the recordings\. The following example shows the general format of an Amazon S3 path with the default prefix\. This example uses `myRecordingBucket` as the `Location` value\.

```
myRecordingBucket/Amazon-Chime-SMA-Call-Recordings/2019/03/01/2019–03–01–17–10–00–010_c4640e3b–1478–40fb-8e38–6f6213adf70b_7ab7748e–b47d–4620-ae2c–152617d3333c.wav
```

The following example shows the data represented in the call recording Amazon S3 path\.

```
s3Bucket/Amazon-Chime-SMA-Call-Recordings/year/month/date/year-month-date-hour-minute-second-millisecond_transactionId_callId.wav
```

When you provide the Amazon S3 bucket and object key prefix in the `Location` field, the SIP media application uses your object\-key prefix in the destination Amazon S3 path instead of the default prefix\. The following example shows the general format of a call recording Amazon S3 path with your prefix\. For example, you can specify myRecordingBucket/technicalSupport/english as the `Location`\. 

```
myRecordingBucket/technicalSupport/english/2019/03/01/2019–03–01–17–10–00–010_c4640e3b1478–40fb–8e38-6f6213adf70b_7ab7748e–b47d–4620–ae2c–152617d3333c.wav
```

The following example shows the data in the Amazon S3 path\.

```
s3Bucket/yourObjectKeyPrefix/year/month/date/year-month-date-hour-minute-second-millisecond_transactionId_callId.wav
```

The recording sent to your Amazon S3 bucket contains additional [ Amazon S3 object metadata](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingMetadata.html) about the call leg\. The following table lists the supported Amazon S3 object metadata\.


| Name | Description | 
| --- | --- | 
| transaction\-id | Transaction ID of the phone call | 
| call\-id | CallId of participant in the CallDetails of the AWS Lambda function invocation | 
| recording\-duration | Call recording duration in seconds | 
| recording\-audio\-file\-format | Call recording audio file format represented as internet media type | 

## Granting Amazon S3 bucket permissions<a name="grant-s3-perms"></a>

Your destination Amazon S3 bucket must belong to the same AWS account as your application\. In addition, the action must give `s3:PutObject` and `s3:PutObjectAcl` permission to the Amazon Chime Voice Connector service principal, `voiceconnector.chime.amazonaws.com`\. The following example grants the appropriate permission\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SIP media applicationRead",
            "Effect": "Allow",
            "Principal": {
                "Service": "voiceconnector.chime.amazonaws.com"
            },
            "Action": [                
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": "arn:aws:s3:::bucket-name/*"
        }
    ]
}
```

## Action successful response<a name="action-successful"></a>

When the call recording is successfully started on a call leg, the SIP media application invokes an AWS Lambda function with the `ACTION_SUCCESSFUL` event type\. The location of call recording is returned in the response\. 

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type" : "StartCallRecording",
        "Parameters": {
            "CallId": "call-id-1",
            "Track": "BOTH",
            "Destination": {
                "Type": "S3",
                "Location": "valid-bucket-name"
            }
        }
        "CallRecordingDestination": {
            "Type": "S3",
            "Location": "call-recording-bucket-and-key"
        }
    }
    "CallDetails": {
        ...
    }
}
```

## Action error response<a name="action-error"></a>

For validation errors, the SIP media application calls the AWS Lambda function with the appropriate error message\. The following table lists the error messages\.




| Error | Message | Reason | 
| --- | --- | --- | 
| `InvalidActionParameter` | `CallId` parameter for action is invalid | Any parameter is invalid\. | 
| `SystemException` | System error while executing an action\. | Another type of system error occurred while executing an action\. | 

When the action fails to record the media on a call leg, the SIP media application invokes an AWS Lambda function with the `ActionFailed` event type\. 

The following example shows a typical error response\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 5,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type" : "StartCallRecording",
        "Parameters": {
            "CallId": "call-id-1",
            "Track": "BOTH",
            "Destination": {
                "Type": "S3",
                "Location": "valid-bucket-name"
            }
        }
        "Error": "NoAccessToDestination: Error while accessing destination"
    }
    "CallDetails": {
        ...
    }
}
```