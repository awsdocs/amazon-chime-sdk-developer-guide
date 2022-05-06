# Ending a call<a name="case-5"></a>

You can use the [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateSipMediaApplicationCall.html) API to end an outbound call\. The API invokes the endpoint of a specified **SIP media application ID**\. Customers can control the flow of the call by returning actions to the SIP media application\.

In the event of a successful response, the API returns a 202 http status code along with the `transactionId`, which you can use with the [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API to update an in\-progress call\.

The following diagram shows the invocations made to the AWS Lambda function endpoint for an outbound call\.

![\[The flow of data when you invoke the CreateSipMediaApplicationCall API. The API invokes a different endpoint when the status of an outbound call changes.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/sip-api-1.png)

The endpoint configured for the SIP media application is invoked for different statuses of the outbound call\. When a customer ands a call, Amazon Chime SDK invokes the endpoint with a `HANGUP` invocation event type\. 

This example shows a typical invocation event for a `HANGUP`\.

```
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