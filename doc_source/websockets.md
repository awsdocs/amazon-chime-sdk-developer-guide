# Using WebSockets to receive messages<a name="websockets"></a>

 You can use the [Amazon Chime JS SDK](https://github.com/aws/amazon-chime-sdk-js) to receive messages using WebSockets, or you can use the WebSocket client library of your choice\.

Follow these steps for using WebSockets:

1. [Define an IAM policy](#define-iam-policy)

1. [Retrieve the endpoint](#retrieve-endpoint)

1. [Establish the connection](#connect-api)

1. [Process the events](#process-events)

## Define an IAM policy<a name="define-iam-policy"></a>

To start, define an IAM policy that gives you permission to establish a WebSocket connection\. The following example policy gives an `AppInstanceUser` permission to establish a WebSocket connection\.

```
"Version": "2012-10-17",
"Statement": [
  {
    "Effect": "Allow",
    "Action: [
      "chime:Connect"
    ],
    "Resource": [
      "arn:aws:chime:region:{aws_account_id}:app-instance/{app_instance_id}/user/{app_instance_user_id}"
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

## Retrieve the endpoint<a name="retrieve-endpoint"></a>

These steps explain how to retrieve the endpoint used in a WebSocket connection\.

1. Use the [ GetMessagingSessionEndpoint ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_GetMessagingSessionEndpoint.html) API to retrieve the WebSocket endpoint\. 

1. Use the URL returned by the [ GetMessagingSessionEndpoint ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_GetMessagingSessionEndpoint.html) API to construct a Signature Version 4 Signed WebSocket URL\. If you need help doing that, you can follow directions in the [Establish the connection](#connect-api)\.

## Establish the connection<a name="connect-api"></a>

 After you retrieve an endpoint, you use the connect API to establish a WebSocket connection to the Amazon Chime back\-end server and receive messages for an `AppInstanceUser`\. You must use AWS Signature Version 4 to sign requests\. For more information about signing a request, see [ Signing AWS Requests with Signature Version 4 ](https://docs.aws.amazon.com/general/latest/gr/Signature Version 4_signing.html)\.

**Note**  
To retrieve the endpoint, you can invoke the [ GetMessagingSessionEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_GetMessagingSessionEndpoint.html) API\. You can use the websocket client library of your choice to connect to the endpoint\.

**Request Syntax**

```
GET /connect
?X-Amz-Algorithm=AWS4-HMAC-SHA256
&X-Amz-Credential=AKIARALLEXAMPLE%2F20201214%2Fregion%2Fchime%2Faws4_request
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

In addition to your access key ID, this parameter also provides the AWS Region and service—the scope—for which the signature is valid\. This value must match the scope you use in signature calculations\. The general form for this parameter value is:

`<yourAccessKeyId>/<date>/<awsRegion>/<awsService >/aws4_request`

For example:

`AKIAIOSFODNN7EXAMPLE/20201214/us-east-1/chime/aws4_request`

**X\-Amz\-Date**

The date and time format must follow the ISO 8601 standard, and you must format it as `yyyyMMddTHHmmssZ`\. For example, you must convert **08/01/2020 15:32:41\.982\-700** to Coordinated Universal Time \(UTC\) and submit it as `20200801T083241Z`\.

**X\-Amz\-Signed\-Headers**

Lists the headers that you used to calculate the signature\. The following headers are required in the signature calculations:
+ The HTTP host header\.
+ Any x\-amz\-\* headers that you plan to add to the request\.

**Note**  
For added security, sign all the request headers that you plan to include in your request\.

**X\-Amz\-Signatures**

Provides the signature to authenticate your request\. This signature must match the signature that Amazon Chime SDK calculates\. If it doesn't, Amazon Chime SDK denies the request\. For example, `733255ef022bec3f2a8701cd61d4b371f3f28c9f19EXAMPLEd48d5193d7`\.

**X\-Amz\-Security\-Token**

Optional credential parameter if using credentials sourced from the Security Token Service\. For more information about the service, see the [https://docs\.aws\.amazon\.com/STS/latest/APIReference/](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)\.

**SessionId**

Indicates a unique Id for the websocket connection being established\.

**UserArn**

Indicates the identity of the `AppInstanceUser` trying to establish a connection\. The value should be the ARN of the `AppInstanceUser`\. For example, `arn:aws:chime:us%2Deast%2D1:123456789012:app%2Dinstance/694d2099%2Dcb1e%2D463e%2D9d64%2D697ff5b8950e/user/johndoe` 

### Using prefetch<a name="prefetch"></a>

When you establish a websocket connection, you can specify `prefetch-on=connect` in your query parameters to deliver `CHANNEL_DETAILS` events\. The prefectch feature comes with the connect API, and the feature enables users to see an enriched chat view without extra API calls\. Users can:
+ See a preview of the last channel message, plus its timestamp\.
+ See the members of a channel\.
+ See a channel's unread markers\.

After a user connects with the prefetch parameter specified, the user receives the session established event, which indicates the connection has been established\. The user then receives up to 50 `CHANNEL_DETAILS` events\. If the user has less than 50 channels, the connect API prefetches all channels via `CHANNEL_DETAILS` events\. If user has more than 50 channels, the API prefetches the top 50 channels that contain unread messages and the latest `LastMessageTimestamp` values\. The `CHANNEL_DETAILS` events arrive in random order, and you receive events for all 50 channels\.

This example shows how to use `prefetch-on=connect`\.

```
GET /connect
?X-Amz-Algorithm=AWS4-HMAC-SHA256
&X-Amz-Credential=AKIARALLEXAMPLE%2F20201214%2Fregion%2Fchime%2Faws4_request
&X-Amz-Date=20201214T171359Z
&X-Amz-Expires=10
&X-Amz-SignedHeaders=host
&sessionId=sessionId
&prefetch-on=connect
&userArn=appInstanceUserArn
&X-Amz-Signature=db75397d79583EXAMPLE
```

This example shows the response for one channel\. You will receive responses for all 50 channels\.

```
{
   "Headers": { 
        "x-amz-chime-event-type": "CHANNEL_DETAILS", 
        "x-amz-chime-message-type": "SYSTEM" 
        },
   "Payload": JSON.stringify"({
        Channel: [ChannelSummary](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_ChannelSummary.html) 
        ChannelMessages: List of [ChannelMessageSummary](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_ChannelMessageSummary.html)  
        ChannelMemberships: List of [ChannelMembershipSummary](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_ChannelMembershipSummary.html )
        ReadMarkerTimestamp: Timestamp   
    })
}
```

## Process the events<a name="process-events"></a>

In order for an`AppInstanceUser` to receive messages after they establish a connection, you must add them to a channel\. To do that, use the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) API\.

**Note**  
An `AppInstanceUser` always receives messages for all channels that they are a member of as long as they have established a connection successfully\.

And `AppInstanceAdmin` and a `ChannelModerator` do not receive messages on a channel unless you use the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) API to explicitly add them\.

### Message structures<a name="message-structures"></a>

Every WebSocket message that you receive adheres to this format:

```
{
   "Headers": {"key": "value"},
   "Payload": "{\"key\": \"value\"}"
}
```

**Headers**  
Amazon Chime SDK messaging uses a single header key, **x\-amz\-chime\-event\-type**\. The next section lists and describes the header's possible values and payloads\.

**Payload**  
Websocket messages return JSON strings\. The structure of the JSON string depends on the `x-amz-event-type` headers\.The following table lists the possible `x-amz-chime-event-type` values and payloads:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/websockets.html)

### Handling disconnects<a name="handle-disconnects"></a>

Websockets can disconnect due to changes in network connectivity, or when credentials expire\. After you open a WebSocket, the Amazon Chime SDK sends regular pings to the messaging client to ensure it is still connected\. If the connection closes, the client receives a WebSocket close code\. The client can try to reconnect, or not, depending on the close code\. The following tables show the close codes that the client can use to reconnect\.

For 1000 to 4000 closure codes, reconnect only for the following messages:


| Closure codes | Can reconnect | Reason | 
| --- | --- | --- | 
| 1001 | Yes | Normal closure | 
| 1006 | Yes | Abnormal closure | 
| 1011 | Yes | Internal server error | 
| 1012 | Yes | Service restart | 
| 1013 | Yes | Try again later | 
| 1014 | Yes | The server was acting as a gateway or proxy and received an invalid response from the upstream server\. This is similar to the 502 HTTP Status Code\. | 

For 4XXX codes, always reconnect *except* for the following messages:


| Closure codes | Can reconnect | Reason | 
| --- | --- | --- | 
| 4002 | No | Client initiated | 
| 4003 | No | Forbidden | 
| 4401 | No | Not authorized \(deprecated\) | 

When the application uses a close code to reconnect, the application should:

1. Call the [ GetMessagingSessionEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_GetMessagingSessionEndpoint.html) again to obtain a new base URL\. 

1. Refresh the IAM credentials if they've expired\.

1. Connect via the WebSocket\. 

If you use the amazon\-chime\-sdk\-js library, this is handled for you if you implement the the [needsRefresh\(\)](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Credentials.html#needsRefresh-property) property and the [refresh\(\)](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Credentials.html#refresh-property) method\. 