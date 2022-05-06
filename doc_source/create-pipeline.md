# Pipeline creation overview<a name="create-pipeline"></a>

You follow a multi\-step process to create an Amazon Chime SDK media capture pipeline\. The following steps outline the creation process, and provide links to to more information for each step\. 

**To create a media capture pipeline**

1. Use the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) API to create an Amazon Chime SDK meeting\. Optionally, you can specify an AWS Region using the `MediaRegion` parameter\. For more information about choosing a media Region, see [Meeting regions](chime-sdk-meetings-regions.md)\. All regions support media capture\.

1. In the response from the [ CreateMeeting ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) API, note the `MeetingId`\. Pass the `MeetingId` in one of these [ARN formats](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) as the `SourceArn` for the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMediaCapturePipeline.html) API\.

1. Create an Amazon S3Bucket and note the bucket ARN\. Use it as the `SinkArn` parameter for the the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMediaCapturePipeline.html) API\. Make sure the bucket allows the Amazon Chime SDK service to upload artifact files to the bucket\. In addition, the bucket must reside in the same AWS account and Region as the meeting\. For more information see [Creating an Amazon S3 bucket](create-s3-bucket.md)\.
**Note**  
Don't forget to add a policy to your IAM user to grant access to your bucket\.

   Also, if you use a Region that AWS disables by default, you must have an Amazon S3 bucket in that Region\. The next section explains the issue\.

**Using Amazon S3 buckets in disabled Regions**  
By default, AWS disables the following Regions, and you can't host meeting resources in them until you enable them: 
   + Africa \(Cape Town\)
   + Asia Pacific \(Hong Kong\)
   + Asia Pacific \(Jakarta\)
   + Europe \(Milan\)
   + Middle East \(Bahrain\)

   If you use one of those Regions, it must have an Amazon S3 bucket\. This applies even if you use the Amazon S3 APIs to communicate with Regions that aren't blocked by default and already have a bucket\. For more information about enabling blocked regions, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\.

1. Create a service\-linked role named *AWSServiceRoleForAmazonChimeSDKMediaPipelines*\. This allows media capture pipelines to access meetings on your behalf\.

1. Once you have the source and sink, use the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMediaCapturePipeline.html) API to create a capture pipeline\. Once successful, the Amazon Chime SDK creates an attendee that joins and captures the meeting\.

The media capture ends and the attendee quits when you run the [DeleteMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DeleteMediaCapturePipeline.html) API, or when the meeting ends\.