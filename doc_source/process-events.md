# Processing the events<a name="process-events"></a>

For an `AppInstanceUser` to receive messages after they establish a connection, you must add them to a channel\. To do that, use the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) API\.

**Note**  
An `AppInstanceUser` always receives messages for all channels that they belong to\. Messaging stops when the `AppInstance` user disconnects\.

An `AppInstanceAdmin` and a `ChannelModerator` do not receive messages on a channel unless you use the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) API to explicitly add them\.

The following topics explain how to process events\.

**Topics**
+ [Understanding message structures](message-structures.md)
+ [Handling disconnects](handle-disconnects.md)