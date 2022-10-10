# Using elastic channels to host live events<a name="elastic-channels"></a>

Elastic channels support large\-scale chat experiences with up to 1\-million members\. Typical uses include watch parties for sporting or political events\. You can use elastic channels only in the US East \(N\. Virginia\) region\.

An elastic channel consists of a single channel with a common configuration, plus a varying—or *elastic*—number of sub\-channels\. The configuration also includes minimum and maximum thresholds for the members in the sub\-channels\. 

For example, say you create an elastic channel with 100 sub\-channels, and for the sub\-channels you set a low threshold of 500 members and a high threshold of 10,000 members\. When users join this example channel, the system automatically assigns them to a single sub\-channel until the member count exceeds 10,000\. At that point, the system creates a new sub\-channel and adds any new members there\. As users leave, the system deletes sub\-channels and distributes members across the remaining sub\-channels\.

Splitting the audience across sub\-channels makes conversations easier for participants to follow\. Moderators also have reduced workloads, because they only need to watch some of the sub\-channels\. In addition, moderators can use the built\-in tools that elastic channels provide\. For example, moderators can [ban users](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_CreateChannelBan.html) from a channel, [create moderators](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_CreateChannelModerator.html), and use [channel flows](https://docs.aws.amazon.com/chime-sdk/latest/dg/using-channel-flows.html) to automatically moderate all the messages in the channel\.

For more information about Amazon Chime SDK Messaging quotas, refer to [Messaging Quotas](https://docs.aws.amazon.com/general/latest/gr/chime-sdk.html) in the *Amazon Chime General Reference*\.

**Topics**
+ [Prerequisites](#elastic-prereqs)
+ [Elastic channel concepts](#elastic-concepts)
+ [Additional supported features](#additional-features)
+ [Creating elastic channels](create-elastic-channel.md)
+ [Managing elastic channel members](manage-elastic-members.md)
+ [Sending elastic channel messages](send-messages-elastic.md)
+ [Understanding WebSocket system messages in elastic channels](websocket-messages-elastic.md)
+ [Using Kinesis streams to receive system messages](elastic-onboard-streams.md)
+ [Testing elastic channels in our demo app](elastic-testing.md)

## Prerequisites<a name="elastic-prereqs"></a>

You must have the following to use elastic channels\.
+ Knowledge of Amazon Chime SDK Messaging functionality, such as managing channels, and sending and receiving messages\.
+ The ability to invoke the Amazon Chime SDK Messaging APIs\.

## Elastic channel concepts<a name="elastic-concepts"></a>

To use elastic channels effectively, you must understand these concepts\.

**Sub\-channels**  
Elastic channels divide their members into logical containers called sub\-channels\. When you add an `AppInstanceUser` to an elastic channel, the user becomes a member of a sub\-channel\. That user can send and receive messages, but only with other members of that sub\-channel\. The system never allows messages from one sub\-channel to appear in other sub\-channels\.

**Scaling**  
To support user engagement, every sub\-channel must meet a minimum membership requirement\. You provide that value when you create an elastic channel\. As users join or leave an event, the system transfers members to different sub\-channels, which makes the overall channel "elastic\." Sub\-channels run the following scaling actions\.  
+ **SCALE\_OUT** – When a new elastic channel membership request comes in and all sub\-channels are full, the system scales out by creating a new sub\-channel, and then transferring memberships from existing sub\-channels to the new sub\-channel\.
+ **SCALE\_IN** – When a sub\-channel membership count goes below the minimum requirement, and another sub\-channel has the capacity to accommodate all the members of the first sub\-channel, a `SCALE_IN` event transfers those memberships, and then deletes the sub\-channel and all messages\.
If you need to access messages from channels that have been deleted, you must first turn on message streaming\. For more information, refer to [Streaming messaging data](streaming-export.md)\.

**Member transfer**  
This occurs when membership balancing moves an `AppInstanceUser` from one sub\-channel to another\. The `AppInstanceUser` still belongs to the elastic channel after the transfer\. However, the new sub\-channel contains different memberships and messages, so the messages sent by the `AppInstanceUser` after the transfer go to those different members\. Membership balancing does not affect moderator memberships\.

**Note**  
 Elastic channels don't support hidden memberships, membership preferences, and read message timestamps\.

## Additional supported features<a name="additional-features"></a>

Elastic channels also support these messaging features\.
+ [channel prefetch](websockets.md#prefetch-direct)
+ [channel flows](https://docs.aws.amazon.com/chime-sdk/latest/dg/using-channel-flows.html)