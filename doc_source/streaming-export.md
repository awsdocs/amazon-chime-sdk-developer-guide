# Streaming messaging data<a name="streaming-export"></a>

You can configure an `AppInstance` to receive data, such as messages and channel events, in the form of a stream\. You can then react to that data in real time\. Currently, Amazon Chime SDK Messaging only accepts Kinesis streams as stream destinations\. You must have these prerequisites to use Kinesis streams with this feature:
+ Kinesis streams must be in the same AWS account as the `AppInstance`\.
+ A stream must be in the same region as the `AppInstance`\.
+ Stream names have a prefix that starts with `chime-messaging-`\.
+ You must configure at least two shards\. Each shard can receive data up to 1MB per second, so scale your stream accordingly\.
+ You must enable server\-side encryption \(SSE\)\.

**To configure a Kinesis stream**

1. Create one or more Kinesis streams using the prerequisites in the previous section\.

1. Call the [ PutAppInstanceStreamingConfigurations ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_PutAppInstanceStreamingConfigurations.html) API\.

   You can configure one or both of two app instance data types, and you can choose the same stream or separate streams for them\. The data types have the following scopes:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/streaming-export.html)

1. Start reading the data from your configured Kinesis stream\. Remember, events that occur before the configuration in Step 2 are not retroactively streamed\.

**Data format**  
Kinesis outputs records in JSON format with the following fields: `EventType` and `Payload`\. The payload format depends on the `EventType`\. The following table lists the event types and their corresponding payload formats\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/streaming-export.html)