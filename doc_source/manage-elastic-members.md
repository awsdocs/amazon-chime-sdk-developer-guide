# Managing elastic channel members<a name="manage-elastic-members"></a>

To manage the members in an elastic channel, use the [CreateChannelMembership](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_CreateChannelMembership.html), [CreateChannelModerator](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_CreateChannelModerator.html), and [CreateChannelBan](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_CreateChannelBan.html) APIs\. The following information explains how to use them\.

**Channel memberships**  
The `CreateChannelMembership` API creates memberships at the sub\-channel level\. sub\-channels can include moderators and regular members\.  
+ **Moderators** – You can add moderators to multiple sub\-channels\. That allows the moderators to send messages on each of the sub\-channels they belong to\. When you add a moderator to a sub\-channel, you must provide the `SubChannelId`\.

  If you want to assign moderators to new sub\-channels automatically, you can [enable message streaming](streaming-export.md), listen for sub\-channel creation events, and then create a moderator membership in response to those events\.

  Finally, you can delete moderators from specific sub\-channels, or from all sub\-channels\. You use the [DeleteChannelMembership](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_DeleteChannelMembership.html) API in both cases\. To delete a moderator from a specific sub\-channel, you provide the `SubChannelId`\. If you don't provide an ID for a sub\-channel, the system removes the moderator from all sub\-channels\. Finally, you can use the [ListSubChannels](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_ListSubChannels) API to list the sub\-channels and the number of members in each\.
+ **Regular members** – These comprise the majority of channel memberships\. You can only add a regular member to one sub\-channel\. Also, you can't pass a `SubChannelId` when creating or deleting channel memberships, because the system controls which sub\-channel a membership is created in\.

**Channel moderators**  
The `CreateChannelModerator` API creates moderators at the elastic channel level\. Moderators can view all messages in all sub\-channels\. When you promote a regular member to channel moderator, the system removes all existing channel memberships for that member\. The same happens when you demote a moderator\.

**Channel bans**  
The `CreateChannelBan` API creates bans at the elastic channel level\. A banned `AppInstanceUser` can't belong to any sub\-channel\. When you ban a member, the system removes all the channel memberships for that member\.