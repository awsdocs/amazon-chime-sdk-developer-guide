# Send a channel message with notifications enabled<a name="send-channel-msg-with-notifications"></a>

The [SendChannelMessage](https://docs.aws.amazon.com/chime/latest/APIReference/API_messaging-chime_SendChannelMessage.html) API has an optional `PushNotification` attribute that the Amazon Chime SDK uses to build the push notification to send to Amazon Pinpoint\. Currently, the Amazon Chime SDK only supports the notification title and body fields\. 

The Amazon Chime SDK also supports APNs VoIP pushes\. To send a push notification as an APNs VoIP push, set the type in the `PushNotification` attribute to VOIP\.