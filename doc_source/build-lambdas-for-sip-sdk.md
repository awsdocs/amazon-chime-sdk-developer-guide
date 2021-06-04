# Building Lambda functions for SIP media applications<a name="build-lambdas-for-sip-sdk"></a>

This section of the Amazon Chime SDK explains how to build AWS Lambda functions for use with Amazon Chime SIP media applications\. Developers can use AWS Lambda functions and SIP media applications to build serverless telephone applications for use in any number of solutions\. For example, you can enable calling from a CRM system, create custom phone workflows, or build interactive voice response systems\.

You can implement AWS Lamda and SIP media applications in a matter of minutes, you only need a few lines of code to add voice to your applications, and you don't need in\-depth knowledge of telephony\.

Lambda functions control the behavior of phone requests\. You can specify different signaling and media actions for each request\. The following topics provide a detailed overview and architectural information about SIP media applications, plus example JSON responses with actions that you can return from functions created with AWS Lambda\.

**Note**  
The topics in this section assume that you use AWS Lambda\. For more information about AWS Lambda, see [ Getting started with AWS Lambda ](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)\. 

**Topics**
+ [About using Lambda functions](using-lambda.md)
+ [Basic call flow](call-flow.md)
+ [Adding invocation rules for incoming invitations](add-invocation-rules.md)
+ [SIP media application call architecture](call-architecture.md)
+ [End\-to\-end use case](use-cases.md)
+ [Building the Lambda functions for SIP applications](writing-Lambdas.md)