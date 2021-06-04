# Adding invocation rules for incoming invitations<a name="add-invocation-rules"></a>

Amazon Chime SIP media applications use the concept of a *SIP rule*\. SIP rules allow you to specify the following `trigger types`:
+ *To phone number* from your Amazon Chime inventory
+ *Request URI hostname* of an Amazon Chime Voice Connector
**Note**  
Amazon Chime administrators choose one of those items when they create SIP rules\.

Either of those items trigger a SIP rule\. When the Amazon Chime service receives a call to a To phone number or Request URI hostname, the SIP media application associated with the SIP rule invokes the Lambda function associated with the SIP media application\. The following diagram shows a typical workflow that uses a phone number\.

![\[Diagram of a SIP rule and SIP media application workflow rule that uses a phone number.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SipRule-ph-to-sipApp.png)

In the diagram:
+ The user dials a phone number\. 
+ The rule associated with the phone number triggers a specific SIP media application\. In turn, the ARN invokes Lambda functions that trigger actions, such as playing a welcome message or accepting a meeting PIN\.
**Note**  
The second target shown in the diagram is optional\. If you specify additional targets, they must be set to different AWS Regions\.

The following diagram shows a typical rule that uses a Request URI hostname\.

![\[Diagram of a rule that uses a Request URI Hostname.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SipRule-VC-to-SipApp.png)

In the diagram:
+ The user dials a phone number associated with an Amazon Chime Voice Connector\.
+ The rule associated with the Request URI hostname triggers a specific SIP media application\. In turn, the application invokes Lambda functions that trigger actions, such as playing a welcome message or accepting a meeting PIN\.