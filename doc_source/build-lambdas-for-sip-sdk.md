# Using SIP media applications with AWS Lambda functions<a name="build-lambdas-for-sip-sdk"></a>

This section of the Amazon Chime SDK explains how to use Amazon Chime SIP rules and SIP media applications with AWS Lambda functions\. Developers can use SIP media applications as part of building telephony applications\. For example, you can create interactive voice response systems for dialing into and out of Amazon Chime SDK meetings\.

Your Lambda functions control the behavior of phone calls, such as playing voice prompts, collecting digits, and routing calls\. You can specify different signaling and media actions for each call\. The following topics provide overview and architectural information about SIP media applications and Lambda functions, including an end\-to\-end use case that you can use to start building an Amazon Chime application\.

**Note**  
The topics in this section assume that you use Lambda\. For more information about Lambda, see [Getting started with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)\. Also, to use this section of the Amazon Chime SDK successfully, an Amazon Chime administrator must create at least one SIP rule and one SIP media application\. For more information about completing those tasks, see [Managing SIP media applications](https://docs.aws.amazon.com/chime/latest/ag/manage-sip-applications.html) in the *Amazon Chime Administrator Guide*\.

**Topics**
+ [Understanding SIP rules, SIP media applications, and Lambda functions](using-lambda.md)
+ [Sample call flow](call-flow.md)
+ [Call flow from SIP rule to Lambda function](add-invocation-rules.md)
+ [About using SIP media application call legs](call-architecture.md)
+ [End\-to\-end use case](use-cases.md)
+ [Building the Lambda functions for SIP media applications](writing-Lambdas.md)