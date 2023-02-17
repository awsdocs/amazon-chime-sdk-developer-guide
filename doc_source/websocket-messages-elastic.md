# Understanding WebSocket system messages in elastic channels<a name="websocket-messages-elastic"></a>

The Amazon Chime SDK sends system messages to all connected clients for events that take place in channels\. The following list describes the system messages for elastic channels\.

**Message events**  
Event payloads for elastic channels contain the `subChannelId` field\. Payloads for non\-elastic channels remain the same\.

**Membership events**  
The `CREATE_CHANNEL_MEMBERSHIP` and `DELETE_CHANNEL_MEMBERSHIP` events now have the `subChannelId` field in their payloads\.   
Elastic channels don't support the `BATCH_CREATE_CHANNEL_MEMBERHSIP` event\. When you call the [BatchCreateChannelMembership](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_BatchCreateChannelMembership.html) API, the system sends individual `CREATE_CHANNEL_MEMBERSHIP` events\.  
You can now use the `UPDATE_CHANNEL_MEMBERSHIP` event type to signal changes in membership information\. For example, during a member transfer from one sub\-channel to another, the system sends an `UPDATE_CHANNEL_MEMBERSHIP` event with the new `SubChannelId` in the payload to indicate that the member was transferred\.   
The system only sends the `UPDATE_CHANNEL_MEMBERSHIP` event to the member that was transferred, and not to other members of the sub\-channel\. For this reason, we encourage you to use the [ListChannelMemberships](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_ListChannelMemberships.html) API instead of WebSockets to populate your channel membership rosters\. For more information, refer to [Using WebSockets to receive messages](websockets.md)\. 