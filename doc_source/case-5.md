# Ending a call<a name="case-5"></a>

When your SIP media application receives a hangup on any call leg, the application invokes the Lambda function with the HANGUP invocation event type\. See the following example\.

```
// if LEG-A receives a hangup, such as attendee hangs up
{
    "SchemaVersion": "1.0",
    "Sequence": 6,
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
                "Direction": "Inbound",
                 "To": "+12065551212",
                "From": "+15105550101",
                "StartTimeInMilliseconds": "1597009588",
                "Status": "Disconnected"
            }
        ]
    }
}

// if LEG-B receives a hangup in a bridged call, such as a meeting ending
{
    "SchemaVersion": "1.0",
    "Sequence": 6,
    "InvocationEventType": "HANGUP",
    "ActionData": {
        "Type": "ReceiveDigits",
        "Parameters": {
            "CallId": "call-id-2",
            "ParticipantTag": "LEG-B"
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
                "ParticipantTag": "Leg-A",
                 "To": "+12065551212",
                "From": "+15105550101",
                "Direction": "Inbound",
                "StartTimeInMilliseconds": "1597009588",
                "Status": "Connected"
            },
            {
                "CallId": "call-id-2",
                "ParticipantTag": "Leg-B",
                "To": "+17035550122",
                "From": "SMA",
                "Direction": "Outbound",
                "StartTimeInMilliseconds": "15010595",
                "Status": "Disconnected"
            }
        ]
    }
}
```