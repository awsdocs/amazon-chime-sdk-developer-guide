# End\-to\-end use case<a name="use-cases"></a>

This use case provides example code for receiving a phone call from a PSTN caller, greeting the caller with an audio message, getting the meeting PIN from the caller, playing audio, and joining the caller to the meeting\.

**Invocation events and actions**  
SIP media applications pass invocation events to Lambda functions as JSON objects\. The objects include the invocation event type and any relevant metatdata\. The Lambda function also returns SIP media application actions as JSON objects, and those objects include an action type and any relevant metadata\.

The following table lists the invocation events, and the possible `ActionData.Type`, when you receive an invocation event\.


|  Invocation event  |  ActionData\.Type  | 
| --- | --- | 
|  ACTION\_SUCCESSFUL  |  CallAndBridge ReceiveDigits PlayAudio PlayAudioAndGetDigits  JoinChimeMeeting ModifyChimeMeetingAttendees RecordMeeting  | 
|  ACTION\_FAILED  |  CallAndBridge PlayAudio PlayAudioAndGetDigits ModifyChimeMeetingAttendees RecordMeeting  | 
| HANGUP |  HangUp  | 
|  DIGITS\_RECEIVED  | ReceiveDigits | 

**Note**  
To implement the following use case, you need at least one phone number in your Amazon Chime inventory, a SIP application that uses a Lambda function with an Amazon Resource Name \(ARN\), and a SIP rule that uses the phone number as its trigger\.

When Amazon Chime receives a call on the phone number specified in a rule, the SIP application invokes a Lambda function with the `NEW_INBOUND_CALL` invocation event type\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 1,
    "InvocationEventType": "NEW_INBOUND_CALL",
    "CallDetails": {
        "TransactionId": "transaction-id",
        "AwsAccountId": "aws-account-id",
        "AwsRegion": "us-east-1",
        "SipRuleId": "sip-rule-id",
        "SipApplicationId": "sip-application-id",
        "Participants": [
            {
                "CallId": "call-id-1",
                "ParticipantTag": "LEG-A",
                "To": "+11234567890",
                "From": "+19876543210",
                "Direction": "Inbound",
                "StartTimeInMilliseconds": "159700958834234",
                "Status": "Connected"
            }
        ]
    }
}
```

 You can program the Lambda function to validate call details and store them for future use\. For a `NEW_INBOUND_CALL` event, the Lambda function responds with a set of actions that play a welcome prompt and ask for the meeting PIN\.

Audio files have the following requirements:
+ You must play audio files from an Amazon Simple Storage Service \(S3\) bucket\. The S3 bucket must belong to the same AWS account as the SIP media application\. In addition, you must give the `s3:GetObject` permission to the Amazon Chime Voice Connector service principal—`voiceconnector.chime.amazonaws.com`\. You can use the S3 console or the command\-line interface \(CLI\) to do that\.
+ You must use PCM WAV files of no more than 50 MB in size\. Amazon Chime recommends 8 KHz mono\.
+ The S3 metadata for each WAV file must contain `{'ContentType': 'audio/wav'}`\.

```
{
    "SchemaVersion": "1.0",
    "Actions": [
        {
            "Type" : "PlayAudio",    
            "Parameters" : {
                "CallId": "call-id-1",
                
                "AudioSource": {
                    "Type": "S3",
                    "BucketName": "chime-meetings-audio-files-bucket-name",
                    "Key": "welcome-to-meetings.wav"
                }
            }
        },
        {
            "Type": "PlayAudioAndGetDigits",
            "Parameters" : {
                "ParticipantTag": "LEG-A",
                
                "AudioSource": {
                    "Type": "S3",
                    "BucketName": "chime-meetings-audio-files-bucket-name",
                    "Key": "enter-meeting-pin.wav"
                },
                "FailureAudioSource": {
                    "Type": "S3",
                    "BucketName": "chime-meetings-audio-files-bucket-name",
                    "Key": "invalid-meeting-pin.wav"
                },
                "MinNumberOfDigits": 3,
                "MaxNumberOfDigits": 5,
                "TerminatorDigits": ["#"],
                "InBetweenDigitsDurationInMilliseconds": 5000,
                "Repeat": 3,
                "RepeatDurationInMilliseconds": 10000
            }
        }
    ]
}
```

The SIP media application runs these actions on call leg A\. Assuming the `PlayAudioAndGetDigits` action receives the digits, the SIP media application invokes the Lambda function with the `ACTION_SUCCESSFUL` event type\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 2,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "PlayAudioAndGetDigits",
        "Parameters" : {
            "ParticipantTag": "LEG-A",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "chime-meetings-audio-files-bucket-name",
                "Key": "enter-meeting-pin.wav"
            },
            "FailureAudioSource": {
                "Type": "S3",
                "BucketName": "chime-meetings-audio-files-bucket-name",
                "Key": "invalid-meeting-pin.wav"
            },
            "MinNumberOfDigits": 3,
            "MaxNumberOfDigits": 5,
            "TerminatorDigits": ["#"],
            "InBetweenDigitsDurationInMilliseconds": 5000,
            "Repeat": 3,
            "RepeatDurationInMilliseconds": 10000
        },
        "ReceivedDigits": "12345" // meeting PIN
    },
    "CallDetails": {
        ... // same as in previous event
    }
}
}
```

You can program a Lambda function to identify the caller based on the `CallDetails` data\. You can also validate the meeting PIN received earlier\. Assuming a correct PIN, you then use the [CreateMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html) and [CreateAttendee](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateAttendee.html) APIs to create the Amazon Chime SDK meeting and generate the join token used by the meeting attendee\. The Lambda function responds with the action to join the Amazon Chime meeting\.

```
{
    "SchemaVersion": "1.0",
    "Actions": [
        {
            "Type": "JoinChimeMeeting",
            "Parameters": {
                "JoinToken": "meeting-attendee-join-token"
            }
        }
    ]
}
```

Assuming the `JoinToken` is valid, the SIP media application joins the Amazon Chime meeting and invokes a Lambda function with the `ACTION_SUCCESSFUL` event, where `CallDetails` contains the data from the SIP media application and the Chime Media Service \(`LEG-B`\) 

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type" : "JoinChimeMeeting",
        "Parameters" : {
            "JoinToken": "meeting-attendee-join-token"
        }
    },
    "CallDetails": {
        "TransactionId": "transaction-id", 
        "AwsAccountId": "aws-account-id",
        "AwsRegion": "us-east-1",
        "SipRuleId": "sip-rule-id",
        "SipApplicationId": "sip-application-id",
        "Participants": [
            {
                "CallId": "call-id-1",
                "ParticipantTag": "LEG-A",
                "To": "+11234567890",
                "From": "+19876543210",
                "Direction": "Inbound",
                "StartTimeInMilliseconds": "159700958834234",
                "Status": "Connected"
            },
            {
                "CallId": "call-id-2",
                "ParticipantTag": "LEG-B",
                "To": "SMA",
                "From": "+17035550122",
                "Direction": "Outbound",
                "StartTimeInMilliseconds": "159700958834234",
                "Status": "Connected"
            }
        ]
    }
}
```

If you want to stop running actions on the call or call leg at this point, you can respond with an empty set of actions\.

```
{
    "SchemaVersion": "1.0"
    "Actions": []
}
```

After the caller hangs up, the SIP media application invokes the Lambda function with the `HANGUP` event\. 

```
{
    "SchemaVersion": "1.0",
    "Sequence": 4,
    "InvocationEventType": "HANGUP",
    "ActionData": {
        "Type": "Hangup",
        "Parameters": {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A"
        }
    },
    "CallDetails": {
        "TransactionId": "transaction-id", 
        "AwsAccountId": "aws-account-id",
        "AwsRegion": "us-east-1",
        "SipRuleId": "sip-rule-id",
        "SipApplicationId": "sip-application-id",
        "Participants": [
            {
                "CallId": "call-id-1",
                "ParticipantTag": "LEG-A",
                "To": "+11234567890",
                "From": "+19876543210",
                "Direction": "Inbound",
                "StartTimeInMilliseconds": "159700958834234",
                "Status": "Disconnected"
            },
            {
                "CallId": "call-id-2",
                "ParticipantTag": "LEG-B",
                "To": "SMA",
                "From": "+17035550122",
                "Direction": "Outbound",
                "StartTimeInMilliseconds": "159700958834234",
                "Status": "Disconnected"
            }
        ]
    }
}
```

If you respond to a `Hangup` event with an action, the SIP media application ignores the action if no other `Participants` show a `Status` of `Connected`\.