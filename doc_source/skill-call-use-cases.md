# Use cases for skill calls<a name="skill-call-use-cases"></a>

Developers can use SIP media applications and AWS Lambda functions to build custom telephony solutions\. Your SIP media application's AWS Lambda function controls the behavior of a phone call, such as routing the call to an existing contact center, CRM, or telephone system\. The following use cases illustrate some common usage scenarios\.

**Topics**
+ [Case 1: Connecting to customer engagement infrastructure](#skill-call-case-1)
+ [Case 2: Bridging a call to a PSTN](#skill-call-case-2)
+ [Case 3: Bridging a call to a meeting](#skill-call-case-3)

## Case 1: Connecting to customer engagement infrastructure<a name="skill-call-case-1"></a>

SIP media applications enable you to integrate skill calling with a customer engagement infrastructure\. You can build a consistent experience for your callers regardless of whether they use a skill call, a PSTN call, a web site, or a mobile application\.

The following diagram shows the data flow for this use case\. Text below the diagram corresponds to the numbers in the image\.

 ![\[Diagram showing the program flow of an Alexa skill call.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/alexa-call-customer-engagement.png) 

In the diagram:

1. A caller starts a conversation with an Alexa\-enabled device\.

1. The device calls the Alexa Service to process the voice request\. The Skill gathers relevant data configured by the Skill developer, such as the caller name, account ID, and reason for the call\. The Skill also attaches the identifier for the data to the call\.

1. The Alexa Service calls the Skill's AWS Lambda function to process the voice request\. 

1. The caller instructs the Skill to start a call\. The Alexa Service invokes the [StartCommunicationSession](communication-session-reference.md#start-communication-session) API\. The call context identifier to these relevant data is attached to the communication session\.

1. The Alexa Service instructs the Alexa device to send a Session Initiation Protocol \(SIP\) Invite to the SIP media application\. 

1. The SIP media application AWS Lambda function responds with a [CallAndBridge](call-and-bridge.md) action\. The action instructs the PSTN Audio service to bridge the skill call to your Amazon Chime SDK Voice Connector\.

1. The SIP media application routes the call to the Amazon Chime SDK Voice Connector\. 

1. The Voice Connector routes the call to the customer engagement infrastructure\.

1. The customer engagement system uses the call context ID in the SIP\_INVITE to load the data collected by the Skill\. The data optimizes the communication workflow and helps callees understand the purpose of the call\.

## Case 2: Bridging a call to a PSTN<a name="skill-call-case-2"></a>

You can use the PSTN Audio service to route skill calls to PSTN phone numbers\. The SIP media application's AWS Lambda function determines the phone number to which the skill call is routed\. For example, your customer may want to speak to a professional located at your closest branch office\.

The following diagram shows the data flow through this use case\. Text below the diagram corresponds to numbers in the image\.

![\[The flow of data through a skill call involving an Amazon Chime SDK Voice Connector.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/alexa-call-pstn.png)

In the diagram:

1. A caller starts a conversation with an Alexa\-enabled device\.

1. The device calls the Alexa Service to process the voice request\. The Skill gathers the data that you configured, such as the caller name, account ID, and reason for the call\. The Skill also attaches the identifier for the data to the call\.

1. The Alexa Service calls your Skill's AWS Lambda function to process the voice request\.

1. The Alexa Service invokes the [StartCommunicationSession](communication-session-reference.md#start-communication-session) API\.

1. The Alexa Service instructs the Alexa device to send a Session Initiation Protocol \(SIP\) Invite to the SIP media application\. 

1. The SIP media application invokes its associated AWS Lambda function, which uses the call context ID to load the call context data and send it to the back\-end cloud service\. The service determines the destination phone number that best serves the customer\. Once the destination phone number is determined, the SIP media application's AWS Lambda function responds with a [CallAndBridge](call-and-bridge.md) action to the SIP media application\. The action's `CallIdNumber` field must use a phone number that belongs to you\. You cannot use the “From” phone number of the skill call in that field\.

1. The SIP media application calls the destination PSTN phone number\.

## Case 3: Bridging a call to a meeting<a name="skill-call-case-3"></a>

SIP media applications enable you to bridge a skill call with an Amazon Chime SDK meeting by providing the attendee join token\. The following diagram shows the data flow through this use case\. Text below the diagram corresponds to numbers in the image\.

![\[The flow of data through a skill call that bridges to an Amazon Chime SDK meeting.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/alexa-bridge-to-meeting.png)

In the diagram:

1. A caller starts a conversation with an Alexa\-enabled device\.

1. The device calls the Alexa Service to process the voice request\. The Skill gathers the data that you configured, such as the caller name, account ID, and reason for the call\. The Skill also attaches the identifier for the data to the call\.

1. The Alexa Service calls your Skill's AWS Lambda function to process the voice request\.

1. The Alexa Service invokes the [StartCommunicationSession](communication-session-reference.md#start-communication-session) API\.

1. The Alexa Service instructs the Alexa device to send a Session Initiation Protocol \(SIP\) Invite to the SIP media application\.

1. The SIP media application invokes its associated AWS Lambda function\. You make Amazon Chime SDK calls to the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) and [CreateAttendee](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateAttendee.html) APIs to get the join token and pass it on in the [JoinChimeMeeting](https://docs.aws.amazon.com/chime-sdk/latest/dg/join-chime-meeting.html) action\.

1. The SIP media application bridges the skill call to the meeting\.