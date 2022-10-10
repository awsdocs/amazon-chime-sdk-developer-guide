# PSTN audio service glossary<a name="chm-dg-glossary"></a>

\| [A](#a) \| [C](#c) \| [E](#e) \| [I](#i) \| [L](#l) \| [M](#m) \| [N](#n) \| [O](#o) \| [P](#p) \| [S](#s) \| [T](#t) \| [V](#v) \| 

## A<a name="a"></a>

**Action**  
In an AWS Lambda function, an action is an item that you want to run on a leg of a phone call, such as sending or receiving digits, joining a meeting, and so on\. For more information about actions supported by the PSTN Audio Service, see [Supported actions for the PSTN Audio service](specify-actions.md)\.

**AWS Lambda**  
A compute service that lets you run code for almost any type of application or backend service without provisioning or managing servers\.

**AWS Lambda function**  
In the context of the PSTN Audio service, a function run in response to data passed by a SIP media application, such as placing an outbound call\. 

## C<a name="c"></a>

**Call detail record**  
Data from Amazon Chime Voice Connector calls, such as account IDs, source phone numbers, and destination countries\. The records land as objects in an Amazon Simple Storage Service \(S3\) bucket in your account\. For more information, see [Managing global settings in Amazon Chime SDK](https://docs.aws.amazon.com/chime-sdk/latest/ag/manage-global.html) in the *Amazon Chime SDK Administrator Guide* and \. For information about the record schema, see [Using call detail records](attributes.md) in this guide\.

**call ID**  
The ID assigned to the legs of all incoming calls\.

**Call leg**  
A part of a call\. In Amazon Chime SDK applications, calls can come from valid phone numbers, a PSTN, or Amazon Chime Voice Connectors\. For more information, see [About using PSTN Audio service call legs](call-architecture.md) in this guide\. 

**Carrier**  
A company that provides mobile services\. Short for **wireless carrier**\.

**Amazon Chime**  
A unified communications and collaboration service provided by AWS\.

**Amazon Chime SDK**  
A software development kit used by developers to add real\-time media and communications to custom communication applications\. 

## E<a name="e"></a>

**E\.164**  
The only accepted format for phone numbers in the PSTN Audio service\. An ITU\-T recommendation, numbers use a 1\-3 digit country code, followed by a maximum 12\-digit subscriber number\. For example: US: `+14155552671`, UK: `+442071838750 44`, Australia: `+61285993444`\. 

**Endpoint**  
 A hardware device or software service, such as a phone or a unified communications application\. 

**EventBridge**  
A serverless event bus service that allows you to connect your applications to data from a variety of sources\.  
SIP media applications do not send data to EventBridge\. For more information, see [Automating Amazon Chime with EventBridge](https://docs.aws.amazon.com/chime-sdk/latest/ag/automating-chime-with-cloudwatch-events.html) in the *Amazon Chime SDK Administrator Guide*\.

## I<a name="i"></a>

**IVR**  
Interactive Voice Response\. A system that allows people to interact with a computer\-operated phone system by voice recognition or touchtone keypads\.

## L<a name="l"></a>

**Leg**  
See [Call leg](#call-leg)\.

## M<a name="m"></a>

**Media**  
The audio, video, or chat messages available for use during an Amazon Chime SDK meeting\. A custom communications application can contain one or more of each media type\.

**Media Pipeline**  
A mechanism for streaming and capturing audio, video, messages, and events during an Amazon Chime SDK meeting\. For more information, see [Creating Amazon Chime SDK media pipelines](media-pipelines.md) in this guide\.

## N<a name="n"></a>

**Number portability**  
The ability to move phone numbers between phone carriers or unified communication systems\. 

## O<a name="o"></a>

**Origination**  
The process of receiving a call from a PSTN and handing that call off to a VoIP endpoint\. 

## P<a name="p"></a>

**Participant tag**  
An identifier assigned to each call participant, `LEG-A` or `LEG-B`\.

**Policy**  
Amazon Chime SDK requires the following types of policies:  
+ **IAM user policy** – A policy that defines the permissions for Identity and Access Management users\. 
+ **Meeting policy** – A policy that enables one user to control another user’s computer when sharing screens during a meeting, and enables the option for meeting attendees to join meetings by receiving a phone call from Amazon Chime SDK\.

**PSTN**  
Public Switched Telephone Network\. The infrastructure and services that provide phone calling capabilities\.

**PSTN Audio service**  
An Amazon Chime SDK service that enables developers to add audio capabilities to their communication solutions\.

## R<a name="r"></a>

**Routing**  
Apps created using the Amazon Chime SDK use one or more types of routing:  
+ **Network routing** – The process of of selecting a path for traffic in a network, or between or across multiple networks\.
+ **Interactions routing** – The process of ensuring that a call goes to the correct recipient or endpoint\. 
+ **Call routing** – A call management feature that queues and distributes inbound calls to predefined recipients or endpoints\.

## S<a name="s"></a>

**SBC**  
Session border controller\. A network element deployed to protect SIP based voice over Internet Protocol \(VoIP\) networks\.

**Sequence**  
The sequence of events that invoke an AWS Lambda function\. Each time a function is invoked during a call, the sequence is incremented\.

**Service limit/service quota**  
The maximum number of resources, such as meetings, audio streams, or content shares, allowed by the Amazon Chime SDK For more information, see [Audio](meetings-sdk.md#audio) in this guide\.

**SIP**  
Session Initiation Protocol, a signaling protocol used to initiate, maintain, and terminate real\-time sessions that include any combination of voice, video, and messaging applications\. For more information, see [SIP: Session Initiation Protocol](https://www.rfc-editor.org/rfc/rfc3261.html)\.

**SIP headers**  
Parameters in AWS Lambda functions that contain call control data, plus other data such as user account IDs\.

**SIP media application**  
A managed object that passes values from a SIP rule to a target AWS Lambda function\. Developers can call the [CreateSipMediaApplication](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateSipMediaApplication.html) API to create SIP media applications, but they must have administrative permissions to do so\.

**SIP rule**  
A managed object that passes phone numbers for Amazon Chime Voice Connector URIs to a target SIP media application\.

**SIP trunk**  
See [Voice Connector](#voice-connector)\.

**SMA**  
See SIP media application\.

**SMA ID**  
See SIP media application\.

## T<a name="t"></a>

**Telco**  
A telecommunications service provider\.

**Termination**  
The process of ending a call\.

**Transaction**  
A call that contains one or more call legs\. For more information, see [About using PSTN Audio service call legs](call-architecture.md) in this guide\.

**Transaction ID**  
The ID of a transaction that contains multiple call legs\. For more information, see [About using PSTN Audio service call legs](call-architecture.md) in this guide\.

## V<a name="v"></a>

**Voice Connector**  
An object that provides provides Session Initiation Protocol \(SIP\) trunking service for phone systems\. Administrators use the Amazon Chime AWS administrative console to create an manage Voice Connectors\. For more information, see [Managing Amazon Chime Voice Connectors](https://docs.aws.amazon.com/chime-sdk/latest/ag/voice-connectors.html) in the *Amazon Chime SDK Administrator Guide*\.

**Voice Connector group**  
A wrapper that contains multiple Voice Connectors from different AWS Regions\. Groups allow incoming calls to fail over across Regions, which creates a fault\-tolerant mechanism\. For more information, see [Managing Amazon Chime Voice Connector groups](https://docs.aws.amazon.com/chime-sdk/latest/ag/voice-connector-groups.html) in the *Amazon Chime SDK Administrator Guide*\.