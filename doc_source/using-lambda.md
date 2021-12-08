# Understanding phone numbers, SIP rules, SIP media applications, and Lambda functions<a name="using-lambda"></a>

Before you can use the PSTN Audio Service, an Amazon Chime SDK administrator must provision your phone numbers and create managed objects called SIP rules and SIP media applications\. You can use the Amazon Chime console or the AWS SDK to provision phone numbers, and to provision the SIP rule and SIP media application managed objects\.

This image shows the relationship between the managed objects that comprise the PSTN Audio service\. Numbers in the image correspond to numbers in the text below the image\.

![\[Managed objects in the Amazon Chime SDK PSTN Audio Service.\]](http://docs.aws.amazon.com/chime/latest/dg/images/pstn-diagram2.png)

You can assign a SIP rule \(2\) to a Phone number or a Voice Connector \(1\) already provisioned in your PSTN Audio service\. Upon receiving an inbound call to a phone number, or an outbound call request from a Voice Connector, the SIP rule invokes a SIP media application/Lambda function \(4\) which performs a predefined call flow\. To provide multi\-region resiliency, in the SIP rule, you can specify multiple alternative target SIP media applications in different AWS regions \(3\) by order of priority for failover\. If one target fails, the PSTN Audio service tries the next one\. Note, that each alternative target must reside in a different AWS region\. 

For more information about provisioning SIP media applications and rules, see [Managing SIP media applications and rules](https://docs.aws.amazon.com/chime/latest/ag/manage-sip-applications.html) in the *Amazon Chime Administrator Guide*\.