# Configuring attachments<a name="configure-attachments"></a>

The Amazon Chime SDK allows you to use your own storage for message attachments, and include them as message metadata\. Amazon Simple Storage Service \(S3\) is the easiest way to get started with attachments\.

**To use S3 for attachments**

1. Create an S3 bucket to store attachments\.

1. Create an IAM policy for the bucket that allows Amazon Chime SDK users to upload, download, and delete attachments from your S3 bucket\.

1. Create an IAM role for use by your Identity provider to vend credentials to users for attachments\.

The [sample application](https://github.com/aws-samples/amazon-chime-sdk/tree/main/apps/chat) provides an example of how to do this with Amazon S3, Amazon Cognito, and the Amazon Chime SDK\.