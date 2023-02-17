# Using SIP headers<a name="sip-headers"></a>

You can now send and receive a User\-To\-User header, a Diversion header, and custom SIP headers in your AWS Lambda functions when you want to exchange call context information with your SIP infrastructure\. 
+ The User\-to\-User \(UUI\) header can be used to send call control data\. This data is inserted by the application initiating a session and used by the application accepting the session\. It is not used for any basic SIP functionality\. For example, you could use the UUI header in a call center to pass information between agents about a call\.
+ The Diversion header is used to show from where the call was diverted and why\. You can use this header to either see diversion information from other SIP agents, or pass it along\.
+ Custom SIP Headers allow you to pass along any other information you want\. For example, if you want to pass along an account id, you can create an X header called “X\-Account\-Id” and add this information\.

You must prefix your custom SIP headers with `x-`\. The headers are exposed in the AWS Lambda function and received as part of a `NEW_INBOUND_CALL` event during an inbound call\. You can also include these headers in outbound call legs when triggering a [CallAndBridge](call-and-bridge.md) action or the [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateSipMediaApplicationCall.html) API\.

The `Participants` section of a Lambda function contains the `SipHeaders` field\. This field is available when you receive a custom header, or when you populate the `User-to-User` or `Diversion` header\.

This example shows an expected response when an AWS Lambda invocation contains SIP headers\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type":"actionType",
        "Parameters":{
            // Parameters vary by actionType
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
                "SipHeaders": {
                    "X-Test-Value": "String",
                    "User-to-User": "616d617a6f6e5f6368696d655f636f6e6e6563745f696e746567726174696f6e;encoding=hex",
                    "Diversion": "sip:+11234567891@public.test.com;reason=unconditional"
                }
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

The following example shows a successful [CallAndBridge](call-and-bridge.md) action, due to an not valid entry for the `SipHeaders` parameter\. 

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "CallAndBridge",
            "Parameters":{
            "CallTimeoutSeconds": 30,
            "CallerIdNumber": "e164PhoneNumber", // required
            "RingbackTone": { // optional
                "Type": "S3",
                "BucketName": "s3_bucket_name",
                "Key": "audio_file_name"
            },
            "Endpoints":[
               {
                    "Uri":"e164PhoneNumber", // required
                    "BridgeEndpointType":"PSTN" // required
               }
            ],
            "SipHeaders": {
                "X-Test-Value": "String",
                "User-to-User": "616d617a6f6e5f6368696d655f636f6e6e6563745f696e746567726174696f6e;encoding=hex",
                "Diversion": "sip:+11234567891@public.test.com;reason=unconditional"
            }
         }
      }
   ]
}
```

The following example shows a failed [CallAndBridge](call-and-bridge.md) action caused by an not valid `SipHeaders` parameter\.

```
{
    "SchemaVersion":"1.0",
    "Sequence":3,
    "InvocationEventType":"ACTION_FAILED",
    "ActionData":{
        "Type":"actionType",
        "Parameters":{
            // Parameters vary by Action Type
            "SipHeaders": {
                "X-AMZN": "String",
                "User-to-User": "616d617a6f6e5f6368696d655f636f6e6e6563745f696e746567726174696f6e;encoding=hex",
                "Diversion": "sip:+11234567891@public.test.com;reason=unconditional"
             },
        },
        "ErrorType": "InvalidActionParameter",
        "ErrorMessage": "Invalid SIP header(s) provided: X-AMZN"
   },
   "CallDetails":{
      .....
      "Participants":[
         {
            "CallId":"call-id-1",
            "ParticipantTag":"LEG-A",
            .....   
            "Status":"Connected"
         },
         {
            "CallId":"call-id-2",
            "ParticipantTag":"LEG-B",
            .....
            "Status":"Connected"
         }
      ]
   }
}
```

## Using the sip\-headers field<a name="custom-headers"></a>

When you trigger the [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateSipMediaApplicationCall.html) API, the optional `SipHeaders` field allows you to pass custom SIP headers to an outbound call leg\. Valid header keys must include one of the following: 
+ The `x-` prefix
+ The `User-to-User` header
+ The `Diversion` header

`X-AMZN` is a reserved header\. If you use this header in an API call, it will fail\. The headers can be a maximum length of 2048 characters\. 

The following example shows a typical [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateSipMediaApplicationCall.html) API in the command\-line interface with the optional `SipHeaders` parameter\.

```
create-sip-media-application-call
    --from-phone-number value // (string)
    --to-phone-number value // (string)
    --sip-media-application-id value // (string)
    --sip-headers // (map)
```

For more information, see [A Mechanism for Transporting User\-to\-User Call Control Information in SIP](https://datatracker.ietf.org/doc/html/rfc7433) and [Diversion Indication in SIP](https://datatracker.ietf.org/doc/html/rfc5806)\.