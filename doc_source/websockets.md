# Using websockets to receive messages<a name="websockets"></a>

 You can use the [Amazon Chime JS SDK](https://github.com/aws/amazon-chime-sdk-js) to receive messages using websockets, or you can also use another websocket client library of your choice\.

**Topics**
+ [Establishing a connection](#establish-connection)
+ [Receiving messages](#receive-messages)
+ [Message structures](#message-structures)
+ [Using the Connect API](#connect-api)

## Establishing a connection<a name="establish-connection"></a>

Follow these steps to establish a websocket connection:

1. Define an IAM policy that gives you permission to establish a websocket connection\. The following example policy gives an `AppInstanceUser` permission to establish a websocket connection\.

   ```
   "Version": "2012-10-17",
   "Statement": [
     {
       "Effect": "Allow",
       "Action: [
         "chime:Connect"
       ],
       "Resource": [
         "arn:aws:chime:us-east-1:{awsAccountId}:app-instance/{appInstanceId}/user/{appInstanceUserId}"
       ]
    },
    {
       "Effect": "Allow",
       "Action: [
         "chime:GetMessagingSessionEndpoint"
       ],
       "Resource": [
         "*"
       ]
    }
    ]
   }
   ```

1. Use the [ GetMessagingSessionEndpoint ](https://docs.aws.amazon.com/chime/latest/APIReference/API_GetMessagingSessionEndpoint.html) API to retrieve the websocket endpoint\. 

1. Use the URL returned by the [ GetMessagingSessionEndpoint ](https://docs.aws.amazon.com/chime/latest/APIReference/API_GetMessagingSessionEndpoint.html) API to construct a Signature Version 4 Signed WebSocket URL\. If you need help doing that, you can follow directions in the [Connect API](#connect-api)\.

## Receiving messages<a name="receive-messages"></a>

In order for an`AppInstanceUser` to start receiving messages after they successfully establish a connection, they must be added to a Channel\. You can add `AppInstanceUsers` to a Channel via the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateChannelMembership.html) API\.

**Note**  
An `AppInstanceUser` always receives messages for all channels that they are a member of as long as they have established a connection successfully\.

And `AppInstanceAdmin` and a `ChannelModerator` do not receive messages on a channel unless you use the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateChannelMembership.html) API to explicitly add them\.

## Message structures<a name="message-structures"></a>

Every WebSocket message that you receive adheres to this format:

```
{
   "Headers": {"Key": "Value"},
   "Payload": "{\"Key\": \"Value\"}"
}
```

**Headers**  
Amazon Chime SDK messaging uses a single header key, **x\-amz\-chime\-event\-type**\. The next section lists and describes the header's possible values and payloads\.

**Payload**  
Websocket messages return JSON strings\. The structure of the JSON string depends on the `x-amz-event-type` headers\.The following table lists the possible `x-amz-chime-event-type` values and payloads:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime/latest/dg/websockets.html)

## Connect API<a name="connect-api"></a>

 Establishes a websocket connection to the back\-end server to receive messages for an `AppInstanceUser`\. The request has to be signed using AWS Signature Version 4\. For more information about signing a request, see [ Signing AWS Requests with Signature Version 4 ](https://docs.aws.amazon.com/general/latest/gr/Signature Version 4_signing.html)\.

**Note**  
To retrieve the endpoint, you can invoke the [ GetMessagingSessionEndpoint](https://docs.aws.amazon.com/chime/latest/APIReference/API_GetMessagingSessionEndpoint.html) API\. You can use the websocket client library of your choice to connect to the endpoint\.

**Request Syntax**

```
GET /connect
?X-Amz-Algorithm=AWS4-HMAC-SHA256
&X-Amz-Credential=AKIARALLEXAMPLE%2F20201214%2Fus-east-1%2Fchime%2Faws4_request
&X-Amz-Date=20201214T171359Z
&X-Amz-Expires=10
&X-Amz-SignedHeaders=host
&sessionId={sessionId}
&userArn={appInstanceUserArn}
&X-Amz-Signature=db75397d79583EXAMPLE
```

**URI Request Parameters**

All URI Request Query Parameters must be URL Encoded\.

**X\-Amz\-Algorithm**

Identifies the version of AWS Signature and the algorithm that you used to calculate the signature\. Amazon Chime supports only AWS Signature Version 4 authentication, so the value of this is `AWS4-HMAC-SHA256`\.

**X\-Amz\-Credential**

In addition to your access key ID, this parameter also provides the AWS region and service—the scope—for which the signature is valid\. This value must match the scope you use in signature calculations\. The general form for this parameter value is:

`<your-access-key-id>/<date>/<AWS-region>/<AWS-service>/aws4_request`

For example:

`AKIAIOSFODNN7EXAMPLE/20201214/us-east-1/chime/aws4_request`

**X\-Amz\-Date**

The date and time format must follow the ISO 8601 standard, and must be formatted as `yyyyMMddTHHmmssZ`\. For example if the date and time was **08/01/2020 15:32:41\.982\-700**, then you must first convert it to UTC \(Coordinated Universal Time\) and then submitted as `20200801T083241Z`\.

**X\-Amz\-Signed\-Headers**

Lists the headers that you used to calculate the signature\. The following headers are required in the signature calculations:
+ The HTTP host header\.
+ Any x\-amz\-\* headers that you plan to add to the request\.

**Note**  
For added security, sign all the request headers that you plan to include in your request\.

**X\-Amz\-Signatures**

Provides the signature to authenticate your request\. This signature must match the signature that Amazon Chime calculates\. If it doesn't, Amazon Chime denies the request\. For example, `733255ef022bec3f2a8701cd61d4b371f3f28c9f19EXAMPLEd48d5193d7`\.

**X\-Amz\-Security\-Token**

Optional credential parameter if using credentials sourced from the STS service\.

**SessionId**

Indicates a unique Id for the websocket connection being established\.

**UserArn**

Indicates the identity of the AppInstanceUser that is trying to establish a connection\. The value should be the ARN of the `AppInstanceUser`\. For example, `arn:aws:chime:us-east-1:123456789012:app-instance/694d2099-cb1e-463e-9d64-697ff5b8950e/user/johndoe` 