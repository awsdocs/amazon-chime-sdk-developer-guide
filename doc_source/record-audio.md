# RecordAudio<a name="record-audio"></a>

Allows the SIP media application to record media from a given call ID\. For example, a voice mail application and meeting\-participant announcements\. The application records until it reaches the duration that you set, or when a user presses one of the `RecordingTerminators`, or when the application detects silence\. In those cases, the action tells your application to put the resulting media file into the specified S3 bucket\. The S3 bucket must belong to the same AWS account as the SIP media application\. In addition, the action must give `s3:PutObject` and `s3:PutObjectAcl` permission to the Amazon Chime Voice Connector service principal, [ Amazon Chime Voice Connector service principal ](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html), `voiceconnector.chime.amazonaws.com`\. 

**Note**  
Recordings made using this feature may be subject to laws or regulations regarding the recording of electronic communications\. It is your and your end users’ responsibility to comply with all applicable laws regarding the recording, including properly notifying all participants in a recorded session or communication that the session or communication is being recorded, and obtaining their consent\.

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
            "Resource": "arn:aws:s3:::bucket-name/*"
        }
    ]
}
```

The following example stops recording when the caller presses the pound key \(\#\), or 10 seconds elapse with no activity, or the caller remains silent for 3 seconds, and it writes the resulting media file into the location defined by `RecordingDestination` parameter\.

**Note**  
This example uses the `CallId` parameter\. You can use the `ParticipantTag` parameter instead, but you can't use both\.

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

****RecordingDestination\.Prefix****  
*Description* – S3 prefix of recording file  
*Allowed values* – A valid prefix name containing up to 979 safe characters\. For more information about safe characters, refer to [Safe characters](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html#object-key-guidelines-safe-characters) in the Amazon Simple Storage Service User Guide\.  
*Required* – No  
*Default* – None\. If not specified, the recording are saved to the root of the S3 bucket\.

**DurationInSeconds**  
*Description* – The duration of the recording, in seconds  
*Allowed values* – >0  
*Required* – No  
*Default value* – None

****SilenceDurationInSeconds****  
*Description* – The duration of silence in seconds, after which the recording stops\. If not specified, silence detection is disabled\.  
*Allowed values* – \[1;1000\]  
*Required* – No  
*Default value* – 200

****SilenceThreshold****  
*Description* – Level of noise that is considered "silence\." If you don't specify `SilenceDurationInSeconds`, this parameter is ignored\.  

**Reference values \(noise levels and thresholds to treat the noise as silence\):**
+ 1—30dB or below, such as a quiet room
+ 100—40\-50 dB, such as a whisper or quiet office
+ 200—60dB, such as a crowded office
+ 1000—75 dB, such as a loud person or music
*Allowed values* – \[1;1000\]  
*Required* – No  
*Default value* – 200

**RecordingTerminators**  
*Description* – Lists all the available recording terminators\.  
*Allowed values* – An array of single digits and symbols from \[123456789\*0\#\]  
*Required* – Yes  
*Default value* – None

## Handling ACTION\_SUCCESSFUL events<a name="handle-action-successful"></a>

When the recording ends, the Amazon Chime SIP media application calls the Lambda function and passes to it the ACTION\_SUCCESSFUL event, along with the invocation results\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": INTEGER,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type" : "RecordAudio",
        "Parameters": {
           ...           
        },
        "RecordingDestination": {
            "Type": "S3",
            "BucketName": "valid-bucket-name",
            "Key": "valid-S3-key"              
        },
        "RecordingTerminatorUsed":"#"
    },
    "CallDetails": {
        ...
    }
}
```

The `ACTION_SUCCESSFUL` event contains `ActionData`, which contains these fields:

**Type**  
*Description* – The type of the action, `RecordAudio`\.

**Parameters**  
*Description* – The parameters of the action\.

**RecordingDestination\.Type**  
*Description* – Type of destination\. Only S3\. 

**RecordingDestination\.BucketName**  
*Description* – The S3 bucket that contains the recording file\. 

**RecordingDestination\.Key**  
*Description* – The S3 key of the recording file\.

**RecordingTerminatorUsed**  
*Description* – The terminator used to stop recording—one of the terminators passed in the `RecordingTerminators` parameter\. If the recording stops after reaching maximum duration \(`DurationInSeconds`\) or because of silence \(`SilenceDurationInSeconds`\), this key\-value pair is not included in the output\.

**Error handling**  
For validation errors, the SIP media application calls the Lambda function with the appropriate error message\. The following table lists the possible error messages\.


|  Error  |  Message  |  Reason  | 
| --- | --- | --- | 
|  `InvalidActionParameter`  |  `CallId` or `ParticipantTag` parameter for action is invalid\. `DurationInSeconds` parameter value is invalid\. `SilenceDurationInSeconds` parameter value is invalid\. `SilenceThreshold` parameter value is invalid\. `RecordingDestination` parameter value is invalid\. Error occurred while uploading recording to S3 bucket\.  |  Any parameter is invalid\.  | 
|  `SystemException`  |  System error while executing an action\.  |  Another type of system error occurred while executing an action\.  | 

## Handling ACTION\_FAILED events<a name="handle-action-failed"></a>

When the action fails to record the media on a call leg, the SIP media application invokes a Lambda function with the `ACTION_FAILED` event type\. See the following example\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 5,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type" : "RecordAudio",
        "Parameters": {
           ...           
        },
        "ErrorType": "InvalidActionParameter",
        "ErrorMessage": "RecordingDestination parameter value is invalid."
    },
    "CallDetails": {
        ...
    }
}
```

## VoiceFocus<a name="voice-focus"></a>

Enables you to apply background noise suppression to inbound and outbound call legs on a public switched telephony network \(PSTN\)\. To create inbound legs, you use a [SIP rule](https://docs.aws.amazon.com/chime/latest/ag/manage-sip-applications.html) that invokes an AWS Lambda function with a `NewInboundCall` event\. You can create outbound call legs by using the [CallAndBridge](call-and-bridge.md) action, or by using a [CreateSIPMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateSipMediaApplicationCall.html) API operation\. For more information about Amazon Voice Focus, see [Using Amazon Voice Focus](https://docs.aws.amazon.com/chime/latest/ug/voice-focus.html) in the *Amazon Chime User Guide*\.

This example shows a typical action\.

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "VoiceFocus",
            "Parameters": {
                "Enable": True|False,            // required
                "CallId": "call-id-1",           
                "ParticipantTag": "LEG-A"
            }
        }
    ]
}
```

**Enable**  
*Description* – Enables or disables Amazon Voice Focus  
*Allowed values* – True, False  
*Required* – Yes  
*Default value* – None

**CallId**  
*Description* – CallId of participant in the `CallDetails` of the Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – No, if you provide a `ParticipantTag`  
*Default value* – None

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`  
*Allowed values* – LEG\-A, LEG\-B  
*Required* – No, if you provide a `CallID`  
*Default value* – None

This example shows a successful `VoiceFocus` action\.

```
{
   "SchemaVersion": "1.0",
   "Sequence": 3,
   "InvocationEventType": "ACTION_SUCCESSFUL",
   "ActionData": {
      "Type": "VoiceFocus",
      "Parameters": {
         "Enable": True,
         "CallId": "call-id-1"
      }
   },
   "CallDetails":{
      .....
      .....
      "Participants":[
         {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            .....   
            "Status": "Connected"
         },
         {
            "CallId": "call-id-2",
            "ParticipantTag": "LEG-B",
            .....
            "Status": "Connected"
         }
      ]
   }
}
```

This example shows an unsuccessful `VoiceFocus` action\.

```
{
   "SchemaVersion": "1.0",
   "Sequence":2,
   "InvocationEventType": "ACTION_FAILED",
      "ActionData":{
      "Type": "VoiceFocus",
      "Parameters": {
         "Enable": True,
         "CallId": "call-id-1"
      }
      },
      "ErrorType": "CallNotAnswered",
      "ErrorMessage": "Call not answered"
   },
   "CallDetails":{
      .....
      .....
      "Participants":[
         {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            .....   
         }
      ]
   }
}
```