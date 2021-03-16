# Adding invocation rules for incoming invitations<a name="add-invocation-rules"></a>

Amazon Chime SIP applications use the concept of a *rule*\. Rules control whether the SIP application connects to a phone number or a Request URI hostname\. Rules are similar to a Public Branch Exchange or Session Border Controller \(PBX/SBC\) dial plan\. The following diagram shows a typical invocation rule that uses a phone number\.

![\[Diagram of a rule that connects to a phone number.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SipRule-ph-to-sipApp.png)

In the diagram:
+ The user dials a phone number\. 
+ The rule associated with the phone number triggers a specific SIP application\. In turn, the ARN invokes Lambda functions that trigger actions, such as playing a welcome message or accepting a meeting PIN\.

The following diagram shows a typical rule that uses a Request URI hostname\.

![\[Diagram of a rule that uses a Request URI Hostname.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SipRule-VC-to-SipApp.png)

In the diagram:
+ The user dials a phone number associated with an Amazon Chime Voice Connector\.
+ The rule associated with the Request URI hostname triggers a specific SIP media application\. In turn, the application invokes Lambda functions that trigger actions, such as playing a welcome message or accepting a meeting PIN\.