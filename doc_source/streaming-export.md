# Streaming export of messaging data<a name="streaming-export"></a>

You can configure your AWS Chime Messaging SDK AppInstance to receive data—messages, channel events, etc\.—in the form of a stream\. Currently, Amazon Chime only accepts Kinesis streams as stream destinations\. Here are the following prerequisites for using your Kinesis streams with this feature:
+ A stream must be in the same region as the AppInstance\.
+ Stream names have a prefix that starts with “chime\-messaging\-”\.
+ You must configure at least two shards\. Each shard can receive data up to 1MB per second, so scale your stream accordingly\.
+ You must enable server\-side encryption \(SSE\)\.

**To configure a Kinesis stream**

1. Create one or more Kinesis streams using the prerequisites in the previous section\.

1. Call the [ PutAppInstanceStreamingConfigurations ](https://docs.aws.amazon.com/chime/latest/APIReference/API_PutAppInstanceStreamingConfigurations.html) API\.

   You can configure two app instance data types\. You can configure one or both types, and you can choose the same stream or separate streams for them\. The data types have the following scopes:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime/latest/dg/streaming-export.html)

1. Start reading the data from your configured Kinesis stream\. Remember, events that occur before the configuration in Step 2 are not retroactively streamed\.

**Data format**  
Kinesis outputs records in JSON format with the following fields: `EventType` and `Payload`\. The payload format depends on the `EventType`\. The following table lists the event types and their corresponding payload formats\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime/latest/dg/streaming-export.html)