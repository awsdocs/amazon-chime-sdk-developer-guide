# Specifying actions in response to telephony events<a name="use-case-2"></a>

In the PSTN Audio service, SIP media applications invoke AWS Lambda functions\. In turn, a Lambda function can return a list of instructions known as *actions*\. An action is an item that you want to run on a leg of a phone call, such as sending or receiving digits, joining a meeting, and so on\. For more information about the actions invoked by the PSTN Audio service, see [Understanding telephony events](pstn-invocations.md)\.

When a SIP media application successfully runs a list of actions, the application calls the AWS Lambda function with an invocation event type of `ACTION_SUCCESSFUL`\. If any of the actions fail to complete, the SIP media application calls the AWS Lambda function with the `ACTION_FAILED` event\.

The SIP media application only returns `ACTION_SUCCESSFUL` if all the actions on the list succeed\. If any of the actions in the list fail, the SIP media application invokes the AWS Lambda function with the `ACTION_FAILED` event and clears the remaining actions in the list after the failed one\. Then the SIP media application runs the next action returned by the AWS Lambda function\. You use the `ActionData` key to identify which call invoked the function\.

The following event shows a sample payload for the `ACTION_SUCCESSFUL` invocation event type after a `PlayAudioAndGetDigits` action\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "PlayAudioAndGetDigits",
        "Parameters" : {
            "CallId": "call-id-1",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "failure-audio-file.wav"
            },
            "FailureAudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "failure-audio-file.wav"
            },
            "MinNumberOfDigits": 3,
            "MaxNumberOfDigits": 5,
            "TerminatorDigits": ["#"],
            "InBetweenDigitsDurationInMilliseconds": 5000,
            "Repeat": 3,
            "RepeatDurationInMilliseconds": 10000
        },
        "ReceivedDigits": "123"
    }
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
                "To": "+12065551212",
                "From": "+15105550101",
                "Direction": "Inbound",
                "StartTimeInMilliseconds": "159700958834234",
                "Status": "Connected"
                }
            ]
        }
    }
}
```

When any action in a list fails to complete successfully, the SIP media application invokes the AWS Lambda function to notify you of the failure, and to get a new set of actions to run on that call\. The following event shows the sample payload for the `ACTION_FAILED` invocation event type after a `PlayAudio` action\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 4,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type": "PlayAudio",
        "Parameters" : {
            "CallId": "call-id-1",
            "AudioSource": {
                "Type": "S3",
                "BucketName": "bucket-name",
                "Key": "audio-file.wav"            
            }
        },
        "ErrorType": "InvalidAudioSource",
        "ErrorMessage": "Audio Source parameter value is invalid."
    }
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
                "To": "+12065551212",
                "From": "+15105550101",
                "Direction": "Inbound",
                "StartTimeInMilliseconds": "159700958834234",
                "Status": "Connected"
            }
        ]
    }
}
}
```