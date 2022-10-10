# Creating Amazon Chime SDK media pipelines<a name="media-pipelines"></a>

**Important**  
You and your end users must understand that recording Amazon Chime SDK meetings may be subject to laws or regulations regarding the recording of electronic communications\. It is your and your end users’ responsibility to comply with all applicable laws regarding the recordings, including properly notifying all participants in a recorded session that the session or communication is being recorded, and obtain their consent\.   
You and your end users are responsible for all content streaming using the media live connector service, and must ensure that such content does not violate the law, infringe or misappropriate the rights of any third party, or otherwise violate a material term of your agreement with Amazon\.

To capture or stream an Amazon Chime SDK meeting, you create media pipelines\. A media pipeline can consist of one of these pipelines: 
+ **Media capture** – You use media capture pipelines to capture audio, video, and content share streams, plus meeting events and data messages\. All media capture pipelines save their data to [Amazon Simple Storage Service](https://aws.amazon.com/s3/) \(S3\) bucket that you create\. You can create one media capture pipeline per Amazon Chime SDK meeting\.
+ **Media concatenation** – You use media concatenation pipelines to concatenate the artifacts from a media capture pipeline\. Concatenation pipelines work independently of media capture and live connector pipelines\. 
+ **Media live connector** – You use media live connector pipelines to connect to services that enable you to stream Amazon Chime SDK meetings to an RTMP endpoint\. You can create up to one media live connector pipeline per Amazon Chime SDK meeting\.

The pipelines you create depend on the namespace you use\. If you use the `Chime` namespace, you can only create media capture pipelines\. If you use the `ChimeSdkMediaPipelines` namespace, you can also create media concatenation and media live connector pipelines, and use compositing features\. If you want to migrate to the `ChimeSdkMediaPipelines` namespace, refer to [Migrating to the ChimeSdkMediaPipelines namespace](migrate-pipelines.md)\.

The following table lists the default limits for active media pipelines in each Region\. Each type of pipeline counts toward the limit\.


| Region | Default active pipeline limit | 
| --- | --- | 
| us\-east\-1 | 100 | 
| us\-west\-2 | 10 | 
| ap\-southeast\-1 | 10 | 
| eu\-central\-1 | 10 | 

**Note**  
If you exceed the limit for any Region, the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaCapturePipeline.html), [CreateMediaConcatenationPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaConcatenationPipeline.html), and [CreateMediaLiveConnectorPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaLiveConnectorPipeline.html) APIs will throw **Resource Limit Exceeded** exceptions\.  
You can use the **Service Quotas** page in the AWS console to adjust your active pipeline limits, or you can contact your [customer support representative](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html)\. For more information about the Amazon Chime SDK meeting limits, see [Amazon Chime SDK service quotas](meetings-sdk.md#mtg-limits)\.

Before you begin, you must integrate your client application with the Amazon Chime SDK client library\. For more information, see [Integrating with a client library](mtgs-sdk-client-lib.md)\. For more information about media pipelines, see [Capture Amazon Chime SDK Meetings Using media pipelines](http://aws.amazon.com/blogs/business-productivity/capture-amazon-chime-sdk-meetings-using-media-capture-pipelines/)\.

**Topics**
+ [Migrating to the ChimeSdkMediaPipelines namespace](migrate-pipelines.md)
+ [Pipeline creation overview](create-pipeline.md)
+ [Creating media capture pipelines](capture-pipe-config.md)
+ [Creating media concatenation pipelines](create-concat-pipe.md)
+ [Creating media live connector pipelines](connector-pipe-config.md)
+ [Enabling compositing for media pipelines](pipeline-compositing.md)
+ [Creating a service\-linked role for Amazon Chime SDK meetings](create-pipeline-role.md)
+ [Using media pipeline events](media-pipe-events.md)
+ [Parsing transcripts](parse-transcripts.md)