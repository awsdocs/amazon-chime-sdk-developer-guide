# Using mobile push notifications to receive messages<a name="using-push-notifications"></a>

You can configure your Amazon Chime Messaging SDK to send channel messages to mobile push notification channels\. The Amazon Chime SDK requires an Amazon Pinpoint application configured for push notifications\. Your Amazon Pinpoint application must meet these prerequisites: 
+ Your Amazon Pinpoint application must have at least an FCM or APNS channel configured and enabled\.
+ Your Amazon Pinpoint application must reside in the same AWS account and region as your Amazon Chime SDK app instance\.

**Topics**
+ [Create an Amazon Pinpoint application](create-pinpoint.md)
+ [Create a service role](create-service-role.md)
+ [Register a mobile device endpoint as an App Instance user](register-endpoint.md)
+ [Send a channel message with notifications enabled](send-channel-msg-with-notifications.md)
+ [Receiving push notifications](receive-notifications.md)
+ [Debugging push notification failures](debug-notifications.md)