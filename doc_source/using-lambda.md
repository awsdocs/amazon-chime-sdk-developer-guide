# Using Lambda functions for SIP applications<a name="using-lambda"></a>

Amazon Chime uses functions created with AWS Lambda for the following reasons:
+ You can add logic to your SIP applications\. For example, you can make decisions based on the output of certain actions, such as an SDK meeting lookup based on a number from a touch\-tone phone, or based on call properties such as a **From** header\.
+ They save time\. You can write lightweight, serverless applications that don't need a server infrastructure\.

When you create a SIP application, you choose an AWS Region\. The application can only use Lambda functions set for that same Region\.