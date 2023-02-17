# Handling skill call events<a name="handle-skill-call-events"></a>

The following example shows a typical Amazon Chime SDK PSTN Audio service event for an inbound skill call\. When the `SIP INVITE` of your skill call reaches your SIP media application, the application invokes its associated AWS Lambda function with this `NEW_INBOUND_CALL` event\. The SIP media application provides the [user\-to\-user](https://datatracker.ietf.org/doc/html/rfc7433) and `X-Alexa-LWA-ClientId` SIP headers to the application's AWS Lambda function\. In turn, that function can use the SIP headers to perform additional validation and processing\.

```
{
  "SchemaVersion": "1.0",
  "Sequence": 1,
  "InvocationEventType": "NEW_INBOUND_CALL",
  "CallDetails":
  {
    "TransactionId": "46e6f593-1f1c-4808-9166-a141624cf145",
    "AwsAccountId": "123456789012",
    "AwsRegion": "us-east-1",
    "SipRuleId": "7c06ea33-b310-4534-9660-207f13284187",
    "SipMediaApplicationId": "d82f98e7-7557-4b6b-a690-3dcfcbf8ab2e",
    "Participants":
    [
      {
        "CallId": "b7b95da2-fe0a-4890-85cc-433cb3931399",
        "ParticipantTag": "LEG-A",
        "To": "+16073331111",
        "From": "+12045551111",
        "Direction": "Inbound",
        "StartTimeInMilliseconds": "1666234046768",
        "SipHeaders":
        {
          "user-to-user": "6373693d64623739323233332d386466;encoding=hex",
          "X-Alexa-LWA-ClientId": "amzn1.application-oa2-client.84ec9912f0be4066be862afaff9d3c48"
        }
      }
    ]
  }
}
```

The `X-Alexa-LWA-ClientId` value is the client ID of the Alexa Skill that initiated the call\. Your SIP media application's AWS Lambda function uses this SIP header to determine that the call is initiated by your Alexa Skill\. 

The user\-to\-user SIP header value is the encoded `clientSessionId` that you provided to the [StartCommunicationSession](communication-session-reference.md#start-communication-session) API\. Your AWS Lambda function decodes the user\-to\-user SIP header to get the `clientSessionId` that you provided in the `StartCommunicationSession` request\. Your SIP media application's AWS Lambda function uses the `clientSessionId` to load the relevant data collected by your Skill from your back\-end cloud service\. 