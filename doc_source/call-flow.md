# Basic call flow<a name="call-flow"></a>

This diagram shows the basic flow of a call through a SIP media application and its associated Lambda functions\. In this case, the application plays a prompt, gathers dual\-tone multi frequency \(DTMF\) numbers, and then ends the call\.

![\[Diagram of basic call flow through a SIP media application and Lambda functions.\]](http://docs.aws.amazon.com/chime/latest/dg/images/lambda-architecture-final2.png)

In the diagram:
+ When users call, the SIP rule associated with the phone number triggers a SIP media application\. In turn, the application invokes Lambda functions that handle the incoming call and return a response\. In this case, an audio file asks the users to enter a meeting PIN\.
+ After the users enter the PIN, they join the meeting\. 
+ When the users hang up\. The SIP media application invokes the Lambda functions that end the call\.