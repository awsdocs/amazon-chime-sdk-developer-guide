# Using the Amazon Chime SDK PSTN Audio Service<a name="build-lambdas-for-sip-sdk"></a>

**Note**  
This section describes the Chime SDK PSTN Audio Service, which was previously referred to as “SIP Media Applications \(SMA\)” in prior versions of the documentation and some blog posts\. Going forward, when we refer to “SIP Media Applications,” we are referring to the configuration items in the Amazon Chime console and the AWS SDK that are associated with the PSTN Audio Service\.

This section explains how to use the Amazon Chime SDK Public Switched Telephone Network \(PSTN\) Audio service\. With the PSTN Audio Service, developers can build custom telephony applications using the agility and operational simplicity of a serverless AWS Lambda function\. 

Your Lambda functions control the behavior of phone calls, such as playing voice prompts, collecting digits, recording calls, routing calls to the PSTN and Session Initiation Protocol \(SIP\) devices using Amazon Chime Voice Connector\. The following topics provide an overview and architectural information about the PSTN Audio Service, including how to build Lambda functions to control calls\. 

**Note**  
The topics in this section assume that you understand the AWS Lambda service\. For more information about Lambda, see [Getting started with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)\. Also, to use this section of the Amazon Chime SDK successfully, an Amazon Chime administrator must create at least one SIP rule and one SIP media application\. For more information about completing those tasks, see [Managing SIP media applications](https://docs.aws.amazon.com/chime/latest/ag/manage-sip-applications.html) in the *Amazon Chime Administrator Guide*\.

**Topics**
+ [Understanding phone numbers, SIP rules, SIP media applications, and Lambda functions](using-lambda.md)
+ [Understanding the PSTN Audio Service programming model](pstn-model.md)
+ [Routing calls and events to Lambda functions](route-calls-events.md)
+ [About using PSTN Audio Service call legs](call-architecture.md)
+ [Sample call flow](call-flow.md)
+ [End\-to\-end use case](use-cases.md)
+ [Building Lambda functions for the PSTN Audio Service](writing-Lambdas.md)