# Receiving push notifications<a name="receive-notifications"></a>

Along with the channel message push notification title and body, the Amazon Chime SDK also includes the channel message ID and channel ARN in the data payload\. You use that information to load the full channel message\.

The following examples shows a typical push notification payload\.

```
{
    "pinpoint.openApp=true",
    "pinpoint.notification.title=PushNotificationTitle",
    "pinpoint.notification.body=PushNotificationBody",
    "pinpoint.campaign.campaign_id=_DIRECT",
    "pinpoint.notification.silentPush=0",
    "pinpoint.jsonBody="{
        "chime.message_id":"ChannelMessageId",
        "chime.channel_arn":"ChannelARN"
    }
}
```

## Disabling or filtering push notification receipts<a name="disable-filter-receipt"></a>

The Amazon Chime SDK provides multiple options to allow app instance users to control whether they wish to receive push notifications\.

**Disabling all push notifications**  
 App instance users can disable push notifications entirely by calling [UpdateAppInstanceUserEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_identity-chime_UpdateAppInstanceUserEndpoint.html) and setting the `AllowMessages` attribute to `NONE`\. 

**Disabling push notifications for a channel**  
App instance users can disable push notifications for a specific channel by calling [PutChannelMembershipPreferences](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_PutChannelMembershipPreferences.html) to `NONE` in the **PushNotification Preferences** field\. 

**Filtering push notifications for a channel**  
App Instance users can set a filter rule so they only receive specific push notifications using the [PutChannelMembershipPreferences](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_PutChannelMembershipPreferences.html) API\. For more information, refer to [Using filter rules to filter messages](filter-msgs.md)\. 