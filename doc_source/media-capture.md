# Creating Amazon Chime media capture pipelines<a name="media-capture"></a>

You create Amazon Chime media capture pipelines when you need to save audio and video artifacts, events, and data messages from Amazon Chime SDK meetings\. The Amazon Chime SDK has a default limit of 100 active media pipelines per account, and you can adjust that limit\.

In the Amazon Chime SDKMediaPipelines namespace, you can create 100 active media pipelines in the us\-east\-1 Region, and 10 pipelines in the us\-west\-2, ap\-southeast\-1, and eu\-central\-1 Regions\. You can adjust those limits\. To do that, contact your customer support representative\. For more information about the Amazon Chime SDK meeting limits, see [Amazon Chime SDK service quotas](meetings-sdk.md#mtg-limits)

Currently, only Amazon Chime SDK meetings support media capture pipelines, and you must use an Amazon Simple Storage Service bucket as a sink for the capture\. Also, before you begin, you must integrate your client application with the AWS SDK client library\. For more information, see [Integrating with a client library](mtgs-sdk-client-lib.md)\. For more information about media capture pipelines, see [Capture Amazon Chime SDK Meetings Using Media Capture Pipelines](http://aws.amazon.com/blogs/business-productivity/capture-amazon-chime-sdk-meetings-using-media-capture-pipelines/)\.

**Important**  
You and your end users understand that recording Amazon Chime SDK meetings with this feature may be subject to laws or regulations regarding the recording of electronic communications\. It is your and your end users’ responsibility to comply with all applicable laws regarding the recordings, including properly notifying all participants in a recorded session that the session or communication is being recorded, and obtain their consent\. 

**Topics**
+ [Migrating to the Amazon Chime SDK Media Pipelines namespace](migrate-pipelines.md)
+ [Pipeline creation overview](create-pipeline.md)
+ [Creating an Amazon S3 bucket](create-s3-bucket.md)
+ [Creating a service\-linked role for Amazon Chime SDK meetings](create-pipeline-role.md)
+ [Enabling server\-side encryption for an Amazon S3 bucket](sse-kms.md)
+ [Media pipeline events](media-capture-events.md)
+ [Sending media pipeline events to CloudTrail](pipeline-cloudtrail.md)
+ [Working with media capture artifacts](artifacts.md)