# Enabling server\-side encryption for an Amazon S3 bucket<a name="sse-kms"></a>

To enable server\-side encryption for an Amazon Simple Storage Service \(Amazon S3\) bucket, you can use these types of encryption keys:
+ An Amazon S3 managed key
+ A customer managed key in the AWS Key Management System \(KMS\)
**Note**  
The Key Management System supports two types of keys, customer managed keys and AWS managed keys\. Amazon Chime SDK meetings only support customer managed keys\. 

## Using an Amazon S3 managed key<a name="s3-keys"></a>

You use the Amazon S3 console, CLI, or REST API to enable server\-side encryption for an Amazon S3 bucket\. In both cases, choose **Amazon S3 Key** as encryption key type\. No further action is needed\. When you use the bucket for media capture, the artifacts are uploaded and encrypted on server side\. For more information, refer to [ Specifying Amazon S3 encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/specifying-s3-encryption.html) in the *Amazon S3 User Guide*\. 

## Using a key that you own<a name="customer-key"></a>

To enable encryption with a key that you manage, you need to enable the Amazon S3 bucket’s server side encryption with a Customer Managed Key, then add a statement to the key policy that allows Amazon Chime to use the key and encrypt any uploaded artifacts\.

1. Create a Customer Managed Key in KMS\. For information about doing so, see [Specifying server\-side encryption with AWS KMS \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/specifying-kms-encryption.html) in the *Amazon S3 User Guide*\.

1. Add a statement to the key policy that allows the `GenerateDataKey` action to generate a key for use by the Amazon Chime SDK service principal, `mediapipelines.chime.amazonaws.com`\.

   This example shows a typical statement\.

   ```
   ...
   {
       "Sid": "MediaPipelineSSEKMS",
       "Effect": "Allow",
       "Principal": {
           "Service": "mediapipelines.chime.amazonaws.com"
       },
       "Action": "kms:GenerateDataKey",
       "Resource": "*",
       "Condition": {
           "StringEquals": {
              "aws:SourceAccount": "Account_Id"
           },
           "ArnLike": {
               "aws:SourceArn": "arn:aws:chime:*:Account_Id:*"
           }
       }
   }
   ...
   ```

1. If you use a media concatenation pipeline, add a statement to the key policy that allows the Amazon Chime SDK service principal, `mediapipelines.chime.amazonaws.com`, to use the `kms:Decrypt` action\.

1. Configure the Amazon S3 bucket to enable server side encryption with the key\.