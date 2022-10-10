# Creating an Amazon S3 bucket<a name="create-s3-bucket"></a>

The Amazon S3 bucket for your media capture pipeline must belong to the same AWS account as the Amazon Chime SDK meeting\. In addition, you must give the `s3:PutObject` and `s3:PutObjectAcl` permission to the Amazon Chime service principal [mediapipelines\.chime\.amazonaws\.com](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)\. You can do that with the Amazon S3 console or the AWS Command Line Interface \(AWS CLI\)\. The S3 bucket must belong to one of the available [Amazon Chime SDK media Regions](https://docs.aws.amazon.com/chime-sdk/latest/dg/sdk-available-regions.html)\.

The following example shows an Amazon S3 bucket policy\.

```
{
    "Version": "2012-10-17",
    "Id": "AWSChimeMediaCaptureBucketPolicy",
    "Statement": [
        {
            "Sid": "AWSChimeMediaCaptureBucketPolicy",
            "Effect": "Allow",
            "Principal": {
                "Service": "mediapipelines.chime.amazonaws.com"
            },
            "Action": [ "s3:PutObject", "s3:PutObjectAcl" ],
            "Resource": "arn:aws:s3:::Bucket_Name/*",
            "Condition": {
                "StringEquals": {
                    "aws:SourceAccount": "Account_Id"
                },
                "ArnLike": {
                    "aws:SourceArn": "arn:aws:chime:*:Account_Id:*"
                }
            }
        }
    ]
}
```