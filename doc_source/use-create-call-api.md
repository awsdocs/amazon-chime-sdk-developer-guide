# Using the CreateSipMediaApplicationCall API<a name="use-create-call-api"></a>

You use the [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateSipMediaApplicationCall.html) API to create an outbound call\. The API invokes the endpoint of a specified **SIP media application ID**\. Customers can control the flow of the call by giving different signaling and SipMediaApplication actions from the endpoint\. 

In the event of a successful response, the API returns a 202 http status code along with a transactionId, which you can use with the [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API to update an in\-progress call\.

The following diagram shows the invocations made to the AWS Lambda function endpoint for an outbound call\.

![\[This is my image.\]](http://docs.aws.amazon.com/chime/latest/dg/images/sip-api-1.png)

The endpoint configured for the SIP media application is invoked for different statuses of the outbound call\. When a customer initiates a call, Amazon Chime invokes the endpoint with a `NEW_OUTBOUND_CALL` invocation event type\. 

This example shows a typical invocation even for a `NEW_OUTBOUND_CALL`\.

```
{
    "SchemaVersion": "1.0",
        "Sequence": 1,
        "InvocationEventType": "NEW_OUTBOUND_CALL",
        "CallDetails": {
            "TransactionId": "transaction-id",
            "AwsAccountId": "aws-account-id",
            "AwsRegion": "us-east-1",
            "SipApplicationId": "sip-application-id",
            "Participants": [
                {
                    "CallId": "call-id-1",
                    "ParticipantTag": "LEG-A",
                    "To": "+1xxxx",
                    "From": "+1xxxxxxx",
                    "Direction": "Outbound",
                    "StartTimeInMilliseconds": "159700958834234"
                }
            ]
    }
}
```

The system ignores any response for an event related AWS Lambda invocation\.

Once a call starts ringing on the receiving end, Chime invokes the configured endpoint again\. 

This example shows a typical invocation event for `RINGING`\.

```
{
    "SchemaVersion": "1.0",
        "Sequence": 1,
        "InvocationEventType": "RINGING",
        "CallDetails": {
            "TransactionId": "transaction-id",
            "AwsAccountId": "aws-account-id",
            "AwsRegion": "us-east-1",
            "SipApplicationId": "sip-application-id",
            "Participants": [
                {
                    "CallId": "call-id-1",
                    "ParticipantTag": "LEG-A",
                    "To": "+1xxxx",
                    "From": "+1xxxxxxx",
                    "Direction": "Outbound",
                    "StartTimeInMilliseconds": "159700958834234"
                }
           ]
    }
}
```

The system ignores any response for an event related AWS Lambda invocation\.

If the call is not answered, or it’s rejected or failed due to an error, Chime disconnects the call and invokes the endpoint with the `Hangup` event type\. For more information about the `Hangup` event type, refer to [Ending a call](case-5.md)\. 

If the call is answered, Chime invokes the endpoint again\. This example shows a typical invocation event for `CALL_ANSWERED`\.

```
{
  "SchemaVersion": "1.0",
    "Sequence": 1,
    "InvocationEventType": "*CALL_ANSWERED*",
    "CallDetails": {
        ""TransactionId": "transaction-id",
            "AwsAccountId": "aws-account-id",
            "AwsRegion": "us-east-1",
            "SipApplicationId": "sip-application-id",
            "Participants": [
                {
                    "CallId": "call-id-1",
                    "ParticipantTag": "LEG-A",
                    "To": "+1xxxx",
                    "From": "+1xxxxxxx",
                    "Direction": "Outbound",
                    "StartTimeInMilliseconds": "159700958834234",
                "Status": "Connected"
            }
        ]
    }
}
```

At this point, you can perform actions on the call by responding to the invocation with an action list\. If you don’t want to perform any actions, just respond with an empty list\. For more information about responding with sets of actions, refer to https://docs\.aws\.amazon\.com/chime/latest/dg/invoke\-on\-call\-leg\.html