# Sending elastic channel messages<a name="send-messages-elastic"></a>

The [SendChannelMessage](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_SendChannelMessage.html) API creates messages at the sub\-channel level\. To send messages, you must have a `subChannelId`\. You can also use the [UpdateChannelMessage](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_UpdateChannelMessage.html), and [RedactChannelMessage](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_RedactChannelMessage.html) APIs to edit and delete messages, but in all cases, you must have a `subChannelId`\.

**Note**  
Message senders can only edit or redact messages if they belong the sub\-channel that they send messages to\. If membership balancing transfers a member to another sub\-channel, that member can only edit or redact the messages that they send in that new sub\-channel\.