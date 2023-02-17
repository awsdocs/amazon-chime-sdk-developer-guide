# Building a media concatenation pipeline<a name="create-concat-pipe-steps"></a>

You follow a multi\-step process to create an Amazon Chime SDK media concatenation pipeline\. The following steps describe the process\.

1. Create an Amazon S3 bucket for use as the media capture pipeline’s data sink, and then configure the bucket policy\. For information about enabling server\-side encryption for the S3 bucket, refer to [Enabling server\-side encryption for an Amazon S3 bucket](https://docs.aws.amazon.com/chime-sdk/latest/dg/sse-kms.html) in this guide\. If you created an Amazon S3 bucket for use with media capture pipelines, you must add the `s3:GetObject` and `s3:ListBucket` actions to that bucket's policy\. The `s3:ListBucket` action requires permission on the bucket\. The other actions require permission on the objects in the bucket\. You must use two different Amazon Resource Names \(ARNs\) to specify bucket\-level and object\-level permissions\.

   The following example shows the bucket policy\. Copy and paste this example as needed\.

   ```
   {
       "Version": "2012-10-17",
       "Id": "AWSChimeMediaCaptureBucketPolicy",
       "Statement": [
           {
               "Sid": "AWSChimeMediaCaptureBucketPolicy",
               "Effect": "Allow",
               "Principal": {
                   "Service": ["mediapipelines.chime.amazonaws.com"]
               },
               "Action": [
                   "s3:PutObject",
                   "s3:PutObjectAcl",
                   "s3:GetObject",
                   "s3:ListBucket",
               ],
               "Resource": [
                   "arn:aws:s3:::[Bucket-Name]/*",
                   "arn:aws:s3:::[Bucket-Name]",
               ],
               "Condition": {
                   "StringEquals": {
                       "aws:SourceAccount": "[Account-Id]"
                   }
               },
               "ArnLike": {
                   "aws:SourceArn": "arn:aws:chime:*:[Account-Id]:*"
               }
           },
       ],
   }
   ```

1.  Create an Amazon S3 bucket for use as the media concatenation pipeline’s data sink, and then configure the bucket policy\. For information about enabling server\-side encryption for the S3 bucket, refer to [Enabling server\-side encryption for an Amazon S3 bucket](https://docs.aws.amazon.com/chime-sdk/latest/dg/sse-kms.html) in this guide\. 

   The following example shows the policy\.

   ```
   {
       "Version": "2012-10-17",
       "Id": "AWSChimeMediaConcatenationBucketPolicy",
       "Statement": [
           {
               "Sid": " AWSChimeMediaConcatenationBucketPolicy ",
               "Effect": "Allow",
               "Principal": {
                   "Service": ["mediapipelines.chime.amazonaws.com"]
               },
               "Action": [
                   "s3:PutObject",
                   "s3:PutObjectAcl"
               ],
               "Resource": "arn:aws:s3:::[Bucket-Name]/*",
               "Condition": {
                   "StringEquals": {
                       "aws:SourceAccount": "[Account-Id]"
                   }
               },
               "ArnLike": {
                   "aws:SourceArn": "arn:aws:chime:*:[Account-Id]:*"
               }
           },
       ]
   }
   ```
**Note**  
You can use a single S3 bucket for media capture and media concatenation pipelines\. However, if you do that, you must add the `s3:GetObject` and `s3:ListBucket` permissions to the media concatenation bucket policy shown in step 2\. If you don't want the concatenation bucket policy to have those permissions, then create separate buckets for each pipeline\. 

1. Use the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaCapturePipeline.html) API to create a media capture pipeline\. As part of that, get the pipeline's ARN\. For information about getting the ARN, refer to [Pipeline creation overview](create-pipeline.md)\. You use the ARN in the next step\.

1. Use the [CreateMediaConcatenationPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaConcatenationPipeline.html) API to create a concatenation pipeline\.

   The following example shows a request body\. The *Path* field is optional, and it defaults to the concatenation pipeline's ID\.
**Note**  
You must use a `MediaPipelineArn` created within the last 30 days\.

   ```
   {
       "Sources": [
           {
               "Type": "MediaCapturePipeline",
               "MediaCapturePipelineSourceConfiguration": {
                   "MediaPipelineArn": "[Media_Pipeline_Arn]",  //must be <30 days old
                   "ChimeSdkMeetingConfiguration": {
                       "ArtifactsConfiguration": {
                           "Audio": {
                               "State": "Enabled"
                           },
                           "Video": {
                               "State": "Enabled | Disabled"
                           },
                           "Content": {
                               "State": "Enabled | Disabled"
                           },
                           "DataChannel": {
                               "State": "Enabled | Disabled"
                           },
                           "TranscriptionMessages": {
                               "State": "Enabled | Disabled"
                           },
                           "MeetingEvents": {
                               "State": "Enabled | Disabled"
                           },
                           "CompositedVideo": {
                               "State": "Enabled | Disabled"
                           }
                       }
                   }
               }
           }
       ],
       "Sinks": [
           {
               "Type": "S3Bucket",
               "S3BucketSinkConfiguration": {
                   "Destination": "arn:aws:s3:::[Bucket_Name]/[Path]"
               }
           }
       ]
   }
   ```

   Concatenation starts whenever the capture pipeline stops\. The concatenation pipeline stops after completing the concatenation\.