# SIP media application call architecture<a name="call-architecture"></a>

SIP media applications can operate on one or more call legs\. For example, you have a single call leg when you record or deliver a voice mail, and you have multiple call legs when you join an Amazon Chime meeting\.

The following diagram shows the flow of a single\-leg call\. In this example, the S3 bucket provides the WAV files that greet and prompt users, and the DynamoDB stores additional data\.

![\[Diagram of the architecture of a one-legged call.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SMA Images-CS-2021-05-05-Single-Leg-Architecture.png)

The following diagram shows the flow of a multi\-leg call\.

![\[Diagram of the architecture of a two-legged, or bridged, call.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SMA Images-CS-2021-05-05-Multi-Leg-Architecture-w-Bridge.png)

Numbers in the following text correspond to numbers in the diagram and describe the call flow\.

1. When the user calls from a private branch exchange \(PBX\) or public carrier, the call signals the SIP media application\. 

1. The application invokes a set of Lambda functions that answer the call and play a media file, such as a welcome message or a request for a PIN\.

1. The application routes the media to the user, who then responds\.

1. The SIP media application invokes the `JoinChimeMeeting` action, which connects the user to the meeting and bridges the call\. 

1. The user sends and receives audio and video during the meeting\.