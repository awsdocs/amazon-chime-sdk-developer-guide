# Using Amazon Chime SDK messaging<a name="using-the-messaging-sdk"></a>

You use this section of the Amazon Chime SDK Developer Guide to help create messaging applications that run on the Amazon Chime SDK service\. This SDK provides the conceptual and practical information needed to create a basic messaging app\.

**Topics**
+ [Messaging prerequisites](#messaging-prerequisites)
+ [Messaging concepts](#messaging-concepts)
+ [Messaging architecture](#messaging-architecture)
+ [Message types](#msg-types)
+ [Messaging quotas](#messaging-quotas)
+ [Getting started](getting-started.md)
+ [Understanding system messages](system-messages.md)
+ [Example IAM roles](iam-roles.md)
+ [Understanding authorization by role](auth-by-role.md)
+ [Streaming messaging data](streaming-export.md)
+ [Using mobile push notifications to receive messages](using-push-notifications.md)
+ [Using filter rules to filter messages](filter-msgs.md)
+ [Using service\-linked roles](using-roles.md)
+ [Using channel flows to process messages](using-channel-flows.md)
+ [Managing message retention](manage-retention.md)
+ [User interface components for messaging](ui-components.md)
+ [Integrating with client libraries](integrate-client-library.md)
+ [Using Amazon Chime SDK messaging with JavaScript](use-javascript.md)

## Messaging prerequisites<a name="messaging-prerequisites"></a>

You need the following to use Amazon Chime SDK messaging\.
+ The ability to program\.
+ An AWS account\.
+ Permissions to configure IAM roles for the applications using Chime SDK messaging\.

For the majority of cases, you also need:
+ **A client application** – Displays messaging UI, connects to web sockets using the Amazon Chime Client SDKs, manages state\.
+ **A server application** – Manages identity and users\.

## Messaging concepts<a name="messaging-concepts"></a>

To use Amazon Chime SDK messaging effectively, you must understand the following terminology and concepts\.

**AppInstance**  
To use Amazon Chime SDK messaging, you must first create an AppInstance\. An AppInstance contains AppInstanceUsers and Channels\. Typically, you create a single AppInstance for your application\. An AWS account can have multiple AppInstances\. You make app level settings, such as message retention and streaming configuration, at the AppInstance level\. AppInstances are identified by a unique ARN in this format: `arn:aws:chime:region:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID`\.

**AppInstanceUser**  
AppInstanceUsers are the entities that send messages, create channels, join channels, and so on\. Typically, you create a one\-to\-one mapping of an `AppInstanceUser` to a user of your app\. You can also create an`AppInstanceUser` to connect to backend services, which allows users to identify messages as coming from a backend service\. AppInstanceUsers identified by an ARN, such as `arn:aws:chime:region:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID/user/APP_INSTANCE_USER_ID`\. You control the `APP_INSTANCE_USER_ID`, and as a best practice, re\-use the IDs that your application already has\.

**Channel**  
When you add an `AppInstanceUser` to a channel, that user becomes a member and can send and receive messages\. Channels can be public, which allows any user to add themselves as a member, or private, which allows only channel moderators to add members\. You can also hide channel members\. Hidden members can observe conversations but not send messages, and they aren't added to the channel membership\.

**UserMessage**  
An `AppInstanceUser` who belongs to a channel can send and receive user messages\. The `AppInstanceUser` can send `STANDARD` or `CONTROL` messages\. `STANDARD` messages can contain 4KB of data and 1KB of metadata\. `CONTROL` messages can contain only 30 bytes of data\. Messages can be `PERSISTENT` or `NON_PERSISTENT`\. You can retrieve `PERSISTENT` messages from the channel history\. `NON_PERSISTENT` messages are only seen by channel members currently connected to Amazon Chime SDK messaging\.

**System Message**  
Amazon Chime SDK generates system messages in response to events such as members joining or leaving a channel\.

## Messaging architecture<a name="messaging-architecture"></a>

You can use Amazon Chime SDK messaging as a server\-side and a client\-side SDK\. The server\-side APIs create an `AppInstance` and `AppInstanceUser`\. You can use various hooks and configurations to add application specific business logic and validation\. For more information about doing that, see [Streaming messaging data](streaming-export.md)\. Additionally, server\-side processes can call APIs on behalf of an `AppInstanceUser`, or control a dedicated `AppInstanceUser` that represents backend processes\.

Client\-side applications represented as an `AppInstanceUser` can call the Amazon Chime SDK messaging APIs directly\. Client\-side applications use the WebSocket protocol to connect to the messaging SDK when they are online\. When connected, they receive real\-time messages from any channel that they are a member of\. When disconnected, an `AppInstanceUser` still belongs to the channels it was added to, and it can load the message history of those channels by using the SDK's HTTP based APIs\.

Client\-side applications have permissions to make API calls as a single `AppInstanceUser`\. To scope IAM credentials to a single `AppInstanceUser`, client side applications assume a parameterized IAM role via AWS Cognito Identity Pools, or by a small self\-hosted backend API\. For more information about authentication, see [Authenticating end\-user client applications](auth-client-apps.md)\. In contrast, server side applications typically have permissions tied to a single app instance user, such as a a user with administrative permissions, or they have permissions to make API calls on behalf of all app instance users\. 

## Message types<a name="msg-types"></a>

You send messages through channels\. You can send `STANDARD`, `CONTROL`, or `SYSTEM` messages\.
+ `STANDARD` messages can be up to 4KB in size and contain metadata\. Metadata is arbitrary, and you can use it in a variety of ways, such as containing a link to an attachment\.
+ `CONTROL` messages are limited to 30 bytes and do not contain metadata\.
+ `STANDARD` and `CONTROL` messages can be persistent or non\-persistent\. Persistent messages are preserved in the history of a channel and viewed by using a `ListChannelMessages` API call\. Non persistent messages are sent to every `AppInstanceUser` connected via WebSocket\.
+ Amazon Chime SDK sends automated `SYSTEM` messages for events such as members joining or leaving a channel\.

## Messaging quotas<a name="messaging-quotas"></a>

The Amazon Chime SDK messaging enforces the following quotas\.


| Resource | Limit | Eligible for increase | 
| --- | --- | --- | 
| App Instances per AWS Account | 100 | Yes | 
| Users per App Instance | 100,000 | Yes | 
| Admins per App Instance | 100 | Yes | 
| Channels per App Instance | 10,000,000 | Yes | 
| Memberships per Channel | 10,000 | Yes**\*** | 
| Moderators per Channel | 1,000 | Yes | 
| Max concurrent connections per user \(Amazon Chime SDK messaging only, does not apply to meetings\) | 10 | Yes | 
| ChannelFlows per App Instance | 100 | Yes | 
| Channel processors in a channel flow | 1 | Yes | 
| Endpoints per App Instance User | 10 | Yes | 

**\***In US\-EAST \(N\. Virginia\)