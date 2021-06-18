# Using Amazon Chime SDK messaging<a name="using-the-messaging-sdk"></a>

You use this SDK to help create messaging applications that run on the Amazon Chime service\. This SDK provides the conceptual and practical information needed to create a basic messaging app\.

**Topics**
+ [Messaging prerequisites](#messaging-prerequisites)
+ [Messaging concepts](#messaging-concepts)
+ [Messaging architecture](#messaging-architecture)
+ [Messaging quotas](#messaging-quotas)
+ [Getting started](getting-started.md)
+ [Understanding system messages](system-messages.md)
+ [Understanding authorization by role](auth-by-role.md)
+ [Streaming export of messaging data](streaming-export.md)
+ [Using service\-linked roles](using-roles.md)
+ [Managing message retention](manage-retention.md)
+ [User interface components for messaging](ui-components.md)
+ [Integrating with client libraries](integrate-client-library.md)
+ [Using Amazon Chime SDK messaging with JavaScript](use-javascript.md)

## Messaging prerequisites<a name="messaging-prerequisites"></a>

You need the following to use Amazon Chime SDK messaging\.
+ The ability to program\.
+ An AWS account\.
+ An IAM role with a policy that grants permission to access the API actions used by the Amazon Chime SDK\. For more information, see [How Amazon Chime works with IAM](https://docs.aws.amazon.com/chime/latest/ag/security_iam_service-with-iam.html) and [ Allow users to access Amazon Chime SDK actions](https://docs.aws.amazon.com/chime/latest/ag/security_iam_id-based-policy-examples.html#security_iam_id-based-policy-examples-chime-sdk) in the *Amazon Chime Administrator Guide*\.

For the majority of cases, you also need:
+ **A server application** – Manages identity and users, and serves those resources to the client application\. The server application is created in the AWS account and must have access to the IAM role mentioned previously\.
+ **A client application** – Displays messaging UI, connects to web sockets using the Amazon Chime Client SDKs, manages state\.

## Messaging concepts<a name="messaging-concepts"></a>

To use Amazon Chime SDK messaging effectively, you must understand the following terminology and concepts\.

**AppInstance**  
To use Amazon Chime SDK messaging, you must first create an `AppInstance`\. Each `AppInstance` is identified by a unique `AppInstanceARN`\. You enable settings, such as message retention and streaming configuration for message export, at the `AppInstance` level\. App instances contain at least one `AppInstanceUser`, plus channels\.

**AppInstanceUser**  
App instance users are resources that represent your users, and they're typically associated with users in your identity provider\. App instance users are represented with unique IDs and ARNs\. An app instance user can be promoted to `ChannelModerator` for elevated privileges in individual channels, or `AppInstanceAdmin` for elevated privileges across all the channels in an app instance\.

**Channel**  
When you add an app instance user to a channel, that user becomes a member and can send and receive messages\. Channels can be public, which allows any user to add themselves as a member, or private, which allows only channel moderators to add members\. You can also hide channel members\. Hidden members can observe conversations but not send message, and they aren't added to channel membership

**ChannelMessage**  
ChannelMessages can be either `STANDARD` or `CONTROL` messages\. `STANDARD` messages can contain 4KB of data and the 1KB of metadata\. `CONTROL` messages can contain only 30 bytes of data\.

**System Message**  
Amazon Chime generates system messages in response to events such as members joining or leaving a channel\.

## Messaging architecture<a name="messaging-architecture"></a>

The Amazon Chime SDK messaging uses the following architecture\.

**App instances, app instance users, and channels**  
App instances contain app instance users and channels\. You configure settings such as message retention policies and streaming export of message data at the app instance level\. You also do the same for most limits\. App instance users can only communicate with other users in the same app instance\.

**Messages**  
Messages are sent in channels, and can be `STANDARD`, `CONTROL`, or `SYSTEM` messages\.
+ `STANDARD` messages can be up to 4KB in size and contain metadata, which can contain a link to an attachment\.
+ `CONTROL` messages are limited to 30 bytes and do not contain metadata\.
+ `STANDARD` and `CONTROL` messages can be persistent or not persistent\.
+ `SYSTEM` messages are automated messages sent by Amazon Chime for events such as members joining or leaving a channel

## Messaging quotas<a name="messaging-quotas"></a>

The Amazon Chime SDK messaging enforces the following quotas\.


| Resource | Limit | Eligible for increase | 
| --- | --- | --- | 
| App Instances Per AWS Account | 100 | Yes | 
| Users per app instance | 100,000 | Yes | 
| App instance admins per app instance | 100 | Yes | 
| Channels Per AppInstance | 10,000,000 | Yes | 
| Memberships Per Channel | 10,000 | Yes | 
| Moderators per channel | 1,000 | Yes | 
| Max concurrent connections per user \(Amazon Chime messaging only, does not apply to meetings\) | 10 | Yes | 