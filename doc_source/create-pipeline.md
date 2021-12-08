# Pipeline creation overview<a name="create-pipeline"></a>

You follow a multi\-step process to create an Amazon Chime media capture pipeline\. The following steps outline the creation process, and provide links to to more information for each step\. 

**To create a media capture pipeline**

1. Use the [CreateMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html) API to create an Amazon Chime SDK meeting\. Optionally, you can specify an AWS Region using the `MediaRegion` parameter\. For more information about choosing a media Region, see [Amazon Chime SDK media Regions](https://docs.aws.amazon.com/chime/latest/dg/chime-sdk-meetings-regions.html)\. All regions support media capture\.

1. In the response from the [ CreateMeeting ](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html) API, note the `MeetingId`\. Pass the `MeetingId` in one of these [ARN formats](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) as the `SourceArn` for the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMediaCapturePipeline) API\.

1. Create an S3 Bucket and note the bucket ARN\. Use it as the `SinkArn` parameter for the the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMediaCapturePipeline) API\. Make sure the bucket allows the Amazon Chime service to upload artifact files to the bucket\. In addition, the bucket must reside in the same AWS account and Region as the meeting\. For more information see [Creating an S3 bucket](create-s3-bucket.md)\.
**Note**  
Do not forget to add a policy to your IAM user in order to grant access to your bucket\.

1. Once you have the source and sink, use the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMediaCapturePipeline) API to create a capture pipeline\. Once successful, the Amazon Chime SDK creates an attendee that joins and captures the meeting\.

The media capture ends and the attendee quits when you run the [DeleteMediaCapturePipeline](https://docs.aws.amazon.com/chime/latest/APIReference/API_DeleteMediaCapturePipeline) API, or when the meeting ends\.