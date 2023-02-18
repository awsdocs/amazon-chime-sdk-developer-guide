# Understanding message structures<a name="message-structures"></a>

Every WebSocket message that you receive adheres to this format:

```
{
   "Headers": {"key": "value"},
   "Payload": "{\"key\": \"value\"}"
}
```

**Headers**  
Amazon Chime SDK messaging use the following header keys:
+ `x-amz-chime-event-type`
+ `x-amz-chime-message-type`
+ `x-amz-chime-event-reason`

The next section lists and describes the header's possible values and payloads\.

**Payload**  
Websocket messages return JSON strings\. The structure of the JSON strings depends on the `x-amz-event-type` headers\. The following table lists the possible `x-amz-chime-event-type` values and payloads:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/message-structures.html)

**x\-amz\-chime\-message\-type**  
The following table lists the `x-amz-chime-message-type` message types \.


| Message type | Description | 
| --- | --- | 
| `STANDARD` | Sent when the websocket recieves a STANDARD channel message\. | 
| `CONTROL` | Sent when the bebsocket receives a CONTROL channel message\. | 
| `SYSTEM` | All other websocket messages sent by Amazon Chime SDK Messaging\. | 

**x\-amz\-chime\-event\-reason**  
This is an optional header supported for a specific use case\. The header provides information about why a specific event was received\.


| Event reason | Description | 
| --- | --- | 
| subchannel\_DELETED | `DELETE_CHANNEL_MEMBERSHIP` events received by elastic channel moderators\. Only seen by moderators after membership balancing deletes a sub\-channel that they belonged to\. | 