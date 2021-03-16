# One\-legged call architecture<a name="handle-one-legged"></a>

Each of your SIP applications invokes a Lambda function\. In turn, your Lambda functions can specify different types of flow actions\. Actions that don't create an outbound leg, such as playing audio or receiving digits, constitute a one\-legged call\. The following diagram shows the architecture of a one\-legged call\.

![\[Diagram of the architecture of a one-legged call.\]](http://docs.aws.amazon.com/chime/latest/dg/images/sma-1-leg-final.png)

Numbers in the following text correspond to numbers in the diagram and describe the call flow\.

1. A user call signals the SIP media application\. 

1. The application invokes a set of Lambda functions that answer the call and play a media file, such as a welcome message or a request for a PIN\.

1. The SIP application routes media to the user\.