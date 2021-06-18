# Call flow from SIP rule to Lambda function<a name="add-invocation-rules"></a>

When Amazon Chime administrators create SIP rules, they specify one of the following `trigger types` to match an incoming call\.
+ **To phone number** – a phone number from your Amazon Chime inventory
+ **Request URI hostname** – the outbound host name of an existing Amazon Chime Voice Connector

A SIP media application invokes its Lambda function when an incoming call matches the trigger type in a SIP rule\. The following diagram shows a typical workflow that uses a "To phone number" trigger type\.

![\[Diagram of a SIP rule and SIP media application workflow rule that uses a "To phone number" trigger type.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SMA Images-CS-2021-05-05-SIP Rules-PSTN-W-Lambda.png)

In the diagram:
+ The user dials a phone number\.
+ The phone number routes to a SIP rule, which the Amazon Chime administrator has already associated with a SIP media application\. In turn, the SIP media application invokes a Lambda function that returns a list of actions, such as playing a welcome message and accepting a meeting PIN\.
+ Optionally, you can use a second SIP media application for redundancy\. If you do, make sure the application uses a different AWS Region\.

The following diagram shows a typical rule that uses a "Request URI hostname" trigger type\.

![\[Diagram of a rule that uses a Request URI Hostname.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SMA Images-CS-2021-05-05-SIP Rules-VC-W-Lambda.png)

In the diagram:
+ The user dials in to an Amazon Chime Voice Connector\. 
+ The Amazon Chime Voice Connector routes to a SIP rule, which the Amazon Chime administrator has already associated with a SIP media application\. In turn, the SIP media application invokes a Lambda function that returns a list of actions, such as playing a welcome message and accepting a meeting PIN\.