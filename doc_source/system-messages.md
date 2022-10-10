# Understanding system messages<a name="system-messages"></a>

Amazon Chime SDK sends system messages to all connected clients for events that take place in channels\. Events include:
+ `UPDATE_CHANNEL` – This event signifies any update made to the channel details, such as the name or metadata\.
+ `DELETE_CHANNEL` – This event signifies that the channel and all of its data, including messages, memberships, moderators and bans, will be deleted\.
+ `CREATE_CHANNEL_MEMBERSHIP` – This event signifies that a particular `AppInstanceUser` has been added as a member to the channel\. The event also contains details of the new `AppInstanceUser`\.
+ `DELETE_CHANNEL_MEMBERSHIP` – This event signifies that an `AppInstanceUser` has been removed from the channel\. The event also contains the removed `AppInstanceUser` details\.
+ `UPDATE_CHANNEL_MEMBERSHIP` – This event only applies to elastic channels\. The event signifies that membership balancing transferred an `AppInstanceUser` from one sub\-channel to another\. The event also contains the `AppInstanceUser` details, plus the information about the sub\-channel that the `AppInstanceUser` was transferred to\.