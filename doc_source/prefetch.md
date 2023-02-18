# Using prefetch to deliver channel details<a name="prefetch"></a>

When you establish a WebSocket connection, you can specify `prefetch-on=connect` in your query parameters to deliver `CHANNEL_DETAILS` events\. The prefectch feature comes with the connect API, and the feature enables users to see an enriched chat view without extra API calls\. Users can:
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