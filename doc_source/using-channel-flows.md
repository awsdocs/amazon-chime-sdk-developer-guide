# Using channel flows to process messages<a name="using-channel-flows"></a>

You use channel flows to run business logic on in\-flight messages before they're delivered to recipients in a messaging channel\. Channel flows can perform actions such as removing government ID numbers, phone numbers, or profanity from messages\. You can also use channel flows to perform functions such as aggregating responses to a poll before sending the results back to participants\.

**Prerequisites**
+ Knowledge of basic Amazon Chime SDK functionality, such as managing channels, and sending and receiving messages\.
+ The ability to invoke the Amazon Chime SDK messaging APIs\.

**Channel flow concepts**

To use channel flows effectively, you must understand these concepts:

**Channel processor**  
An AWS Lambda function that runs preprocessing logic on channel messages\. When you associate a channel with a channel flow, the processor in the flow is invoked for every message in the channel\. To reduce latency, a single processor works best for most use cases\. Finally, each processor must make a callback to the Amazon Chime SDK service once processing completes\.   
We currently only support one processor per channel flow\. If you need more than one processor, submit a support ticket for an increase\.

**Channel flow**  
Channel Flows are containers for up to three channel processors, plus a run sequence\. You associate a flow with a channel, and the processor takes action on all messages sent to that channel\.

**Invoking channel flows**  
The following items invoke channel flows:
+ New persistent standard messages
+ New non\-persistent standard messages
+ Updated persistent standard messages

**Note**  
Channel flows don't process Control or System messages\. For more information about the message types provided by Chime SDK Messaging, refer to [Message types](using-the-messaging-sdk.md#msg-types)\.

**Topics**
+ [Setting up a Channel Processor](processor-setup.md)
+ [Creating a channel flow](create-channel-flow.md)
+ [Associating and disassociating channel flows](associate-channel-flow.md)
+ [Sending messages](sending-msgs.md)
+ [Creating failure alerts by automating with EventBridge](event-brige-events.md)