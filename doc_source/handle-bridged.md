# Two\-legged call architecture<a name="handle-bridged"></a>

When a Lambda function specifies the `JoinChimeMeeting` action for an incoming SIP request from a customer's PBX/SBC or a phone call, Amazon Chime adds a second leg to the Amazon Chime meeting\. The result is a two\-legged call\. The following diagram shows the architecture of a two\-legged, or *bridged*, call\.

![\[Diagram of the architecture of a two-legged, or bridged, call.\]](http://docs.aws.amazon.com/chime/latest/dg/images/sma-2-leg2.png)

Numbers in the following text correspond to numbers in the diagram and describe the call flow\.

1. When the user calls from a private branch exchange \(PBX\) or public carrier, the call signals the SIP media application\. 

1. The application invokes a set of Lambda functions that answer the call and play a media file, such as a welcome message or a request for a PIN\.

1. The application routes the media to the user, who then responds\.

1. The SIP application invokes the `JoinChimeMeeting` action, which connects the user to the meeting and bridges the call\. 

1. The user sends and receives audio and video during the meeting\.