# Pipeline creation overview<a name="create-pipeline"></a>

You follow a multi\-step process to create an Amazon Chime SDK media pipeline\. The following steps outline the creation process, and provide links to more information for each step\. 

**To create a media pipeline**

1. Use the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) API to create an Amazon Chime SDK meeting\. Optionally, you can specify an AWS Region using the `MediaRegion` parameter\. For more information about choosing a media Region, see [Using meeting Regions](chime-sdk-meetings-regions.md)\. 

1. In the response from the [ CreateMeeting ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) API, note the `MeetingId`\. Pass the `MeetingId` in one of these [ARN formats](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) as the `SourceArn` for the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMediaCapturePipeline.html) and [CreateMediaLiveConnectorPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaLiveConnectorPipeline.html) APIs\.

1. Create a service\-linked role named `AWSServiceRoleForAmazonChimeSDKMediaPipelines`\. This allows media pipelines to access meetings on your behalf\.

**Setting permissions**

Calling the media pipeline APIs requires an IAM role with sufficient permission\. To create that role, we recommend adding the [AmazonChimeSDK](https://docs.aws.amazon.com/chime-sdk/latest/ag/security_iam_id-based-policy-examples.html#security_iam_id-based-policy-examples-chime-sdk) managed policy from the IAM console\. The policy contains the necessary APIs\.

To call the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaCapturePipeline.html) or [CreateMediaConcatenationPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaConcatenationPipeline.html) APIs, your IAM role must also have permission to call the S3 [GetBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketPolicy.html) API on all resources\. The following example shows a typical policy for doing so\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "s3:GetBucketPolicy",
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

**To create a media capture pipeline**

1. Create an Amazon S3 bucket and note the bucket ARN\. Use it as the `SinkArn` parameter for the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMediaCapturePipeline.html) API\. Make sure the bucket allows the Amazon Chime SDK service to upload artifact files to the bucket\. In addition, the bucket must reside in the same AWS account and Region as the meeting\. For more information see [Creating an Amazon S3 bucket](create-s3-bucket.md)\.

1. Once you have the source and sink, use the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMediaCapturePipeline.html) API to create a capture pipeline\. Once successful, the Amazon Chime SDK creates an attendee that joins and captures the meeting\.

After you create a media capture pipeline and set its permissions, you create a media concatenation pipeline to concatenate the 5\-second media chunks into a single file\.

**To create a media concatenation pipeline**

1. Add the `s3:GetObject` and `s3:ListBucket` actions to the media capture pipeline S3 bucket policy\.

1. Create a second S3 bucket to use as the concatenation pipeline's data sink\.

1. Call the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaCapturePipeline.html) API to create a media capture pipeline\.

1. Call the [CreateMediaConcatenationPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaConcatenationPipeline.html) API to create a media concatenation pipeline\.
**Note**  
Media capture pipelines, media concatenation pipelines, and S3 buckets must reside in the same AWS account\. Don't forget to add a policy to your IAM user that grants access to your bucket\.

**To create a media live connector pipeline**

1. Acquire the targeted streaming platform’s Real\-Time Messaging Protocol \(RTMP\) or RTMPS URL, a combination of the stream URL and stream key\.

1. Once you have the source and sink, use the [CreateMediaLiveConnectorPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMediaLiveConnectorPipeline.html) API to create a media live connector pipeline\. Once successful, the Amazon Chime SDK creates an attendee that joins and streams the meeting\.

**Note**  
As a best practice for stopping media pipelines, call the [DeleteMediaPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DeleteMediaPipeline.html) API\. The API allows you to delete media capture and media live connector pipelines\. You can also call the [DeleteMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DeleteMediaCapturePipeline.html) API to delete media capture pipelines\. All media pipelines stop when the meeting ends\.