# Using Kinesis streams to receive system messages<a name="elastic-onboard-streams"></a>

You can configure an `AppInstance` to receive data in the form of a stream\. For example, a stream can include messages, sub\-channel events, and channel events\.

As part of that, we support the `CREATE_SUB_CHANNEL` and `DELETE_SUB_CHANNEL` events\. They indicate when a sub\-channel was created or deleted as part of membership balancing\. For more information about receiving data streams, refer to [Streaming messaging data](streaming-export.md)\.