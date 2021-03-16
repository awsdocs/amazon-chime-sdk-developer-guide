# Building Lambda functions for SIP media applications<a name="build-lambdas-for-sip-sdk"></a>

With this SDK, developers can use AWS Lambda to build functions for Amazon Chime SIP media applications\. In turn, Amazon Chime administrators use the Lambda functions when they create SIP media applications\. Your Lambda functions control the behavior of phone requests\. You can specify different signaling and media actions for each request\. The following topics provide a detailed overview and architectural information about SIP applications, plus example JSON responses with actions that you can return from functions created with AWS Lambda\.

**Note**  
The topics in this section assume that you use AWS Lambda\. For more information about AWS Lambda, see [ Getting started with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)\.

**Topics**
+ [Using Lambda functions for SIP applications](using-lambda.md)
+ [Basic call flow](call-flow.md)
+ [Adding invocation rules for incoming invitations](add-invocation-rules.md)
+ [SIP application call architecture](call-architecture.md)
+ [Building the Lambda functions for SIP applications](writing-Lambdas.md)