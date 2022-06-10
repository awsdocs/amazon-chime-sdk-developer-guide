# Messaging concepts<a name="messaging-concepts"></a>

To use Amazon Chime SDK messaging effectively, you must understand the following terminology and concepts\.

**AppInstance**  
To use Amazon Chime SDK messaging, you must first create an AppInstance\. An AppInstance contains AppInstanceUsers and Channels\. Typically, you create a single AppInstance for your application\. An AWS account can have multiple AppInstances\. You make app level settings, such as message retention and streaming configuration, at the AppInstance level\. AppInstances are identified by a unique ARN in this format: `arn:aws:chime:region:aws_account_id:app-instance/app_instance_id`\.

**AppInstanceUser**  
AppInstanceUsers are the entities that send messages, create channels, join channels, and so on\. Typically, you create a one\-to\-one mapping of an `AppInstanceUser` to a user of your app\. You can also create an`AppInstanceUser` to connect to backend services, which allows users to identify messages as coming from a backend service\. AppInstanceUsers identified by an ARN, such as `arn:aws:chime:region:aws_account_id:app-instance/app_instance_id/user/app_instance_user_id`\. You control the `app_instance_user_id`, and as a best practice, re\-use the IDs that your application already has\.

**Channel**  
When you add an `AppInstanceUser` to a channel, that user becomes a member and can send and receive messages\. Channels can be public, which allows any user to add themselves as a member, or private, which allows only channel moderators to add members\. You can also hide channel members\. Hidden members can observe conversations but not send messages, and they aren't added to the channel membership\.

**UserMessage**  
An `AppInstanceUser` who belongs to a channel can send and receive user messages\. The `AppInstanceUser` can send `STANDARD` or `CONTROL` messages\. `STANDARD` messages can contain 4KB of data and 1KB of metadata\. `CONTROL` messages can contain only 30 bytes of data\. Messages can be `PERSISTENT` or `NON_PERSISTENT`\. You can retrieve `PERSISTENT` messages from the channel history\. `NON_PERSISTENT` messages are only seen by channel members currently connected to Amazon Chime SDK messaging\.

**System Message**  
Amazon Chime SDK generates system messages in response to events such as members joining or leaving a channel\.