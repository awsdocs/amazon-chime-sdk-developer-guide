# Overview<a name="alexa-overview"></a>

Alexa skill calling enables your Alexa Skill to place calls directly to your Amazon Chime SDK SIP media application\. The following diagram shows the sequence of an Alexa skill call\. Text below the diagram corresponds to the numbers in the image\.

![\[The flow of data through an Alexa skill call.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/alexa-call-overview.png)

In the diagram:

1. A caller starts a conversation with an Alexa\-enabled device\.

1. The device calls the Alexa Service to process the voice request\.

1. The Alexa Service calls the Skill's AWS Lambda function to process the voice request\.

1. The user instructs the Skill to start a call\. The Alexa Service invokes the [StartCommunicationSession](communication-session-reference.md#start-communication-session) API\.

1. The Alexa Service instructs the Alexa device to send a SIP Invite to the SIP media application\.

1. The SIP media application does the following:
   + Ensures the Skill has permission to call the application's AWS Lambda function\. 
   + Invokes the AWS Lambda function, which responds with a [list of actions](specify-actions.md) that the SIP media application performs during the call\.

To get started, you first use the Amazon Chime SDK console to enable skill calling for at least one SIP media application\. For more information, see [Enabling Alexa calling](https://docs.aws.amazon.com/chime-sdk/latest/ag/enable-alexa-calling.html) in the *Amazon Chime SDK Administrator Guide*\. Until you enable Alexa calling, the SIP media application rejects any calls from a skill\.

Next, you [use the Alexa Developer Console](https://developer.amazon.com/alexa/console/ask) to enable the `Communication - Calling` permission for your Skill\. For more information, see [Enable the Communication \- Calling permission for the Skill](https://docs.aws.amazon.com/chime-sdk/latest/ag/enable-alexa-calling.html)\. Finally, for more information about building custom Skills, see [ Alexa Custom Skills](https://developer.amazon.com/en-US/alexa/alexa-skills-kit/get-deeper/custom-skills) You'll find both topics in the *Alexa Skills Kit*\.

**Important**  
Alexa skill calling is offered to Amazon Chime SDK customers and Alexa Skill developers pursuant to the [Amazon Developer Services Agreement](https://developer.amazon.com/support/legal/da) and the [Program Materials License](https://developer.amazon.com/support/legal/pml)\.