# Understanding authorization by role<a name="auth-by-role"></a>

The tables in this topic list the actions that app instance users can run, depending on their role\.

**Legend**
+ **Allowed** – If the correct Action/Resource context is specified in the IAM Policy, then it can be successfully run\.
+ **Allowed with restrictions** – If correct Action/Resource context is specified in the IAM Policy then certain conditions have to be met to successfully run the action\.
+ **Denied** – Even if correct Action/Resource context is specified in the IAM Policy, it will still be blocked by the back end\.

**Topics**
+ [AppInstanceAdmin](#appinstanceadmin)
+ [ChannelModerator](#channelmoderator)
+ [Member](#member)
+ [Non\-member](#non-member)

## AppInstanceAdmin<a name="appinstanceadmin"></a>

App instance administrators can perform actions on a channels within the app instance for which they are an admin\.


| API name | Allowed or denied | Notes | 
| --- | --- | --- | 
| `UpdateChannel` | Allowed with restriction |  Cannot update ElasticChannelConfiguration once set | 
| `DeleteChannel` | Allowed |  | 
| `DescribeChannel` | Allowed |  | 
| `ListChannel` | Allowed |  | 
| `ListChannelMembershipsForAppInstanceUser` | Allowed | You can also populate [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DescribeChannelModeratedByAppInstanceUser.html#API_DescribeChannelModeratedByAppInstanceUser_RequestSyntax) with another AppInstanceUser\. | 
| `DescribeChannelMembershipForAppInstanceUser` | Allowed | You can also populate [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DescribeChannelModeratedByAppInstanceUser.html#API_DescribeChannelModeratedByAppInstanceUser_RequestSyntax) with another AppInstanceUser\. | 
| `ListChannelsModeratedByAppInstanceUser` | Allowed | You can also populate [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DescribeChannelModeratedByAppInstanceUser.html#API_DescribeChannelModeratedByAppInstanceUser_RequestSyntax) with another AppInstanceUser\. | 
| `DescribeChannelModeratedByAppInstanceUser` | Allowed | You can also populate [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DescribeChannelModeratedByAppInstanceUser.html#API_DescribeChannelModeratedByAppInstanceUser_RequestSyntax) with another AppInstanceUser\. No allowed for elastic channels\. | 
| `CreateChannelMembership` | Allowed |  | 
| `DescribeChannelMembership` | Allowed |  | 
| `ListChannelMembership` | Allowed |  | 
| `DeleteChannelMembership` | Allowed |  | 
| `SendChannelMessage` | Allowed with restriction | You first need to use [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) to create a membership for yourself, and then call the API\. | 
| `GetChannelMessage` | Allowed |  | 
| `ListChannelMessage` | Allowed |  | 
| `DeleteChannelMessage` | Allowed |  | 
| `RedactChannelMessage` | Allowed |  | 
| `UpdateChannelMessage` | Allowed with restriction | You can only edit your own messages\. | 
| `CreateChannelModerator` | Allowed |  | 
| `DeleteChannelModerator` | Allowed |  | 
| `DescribeChannelModerator` | Allowed |  | 
| `ListChannelModerator` | Allowed |  | 
| `CreateChannelBan` | Allowed with restriction | The `AppInstanceUser` that you ban cannot be an `AppInstanceAdmin` or the moderator of that channel\. | 
| `DeleteChannelBan` | Allowed with restriction |  | 
| `DescribeChannelBan` | Allowed |  | 
| `ListChannelBan` | Allowed |  | 
| `UpdateChannelReadMarker` | Allowed with restriction | For non\-elastic channels, You need to use [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) to create a membership for yourself first, and then call the API\. Not allowed for elastic channels\.  | 
|  `GetChannelMessage`  |  Allowed with Restriction | Allowed only for sent messages\. Not allowed for messages in processing by channel flow unless you are the message sender\. | 
| `ListChannelMessages` |  Allowed |  | 
| `DeleteChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. | 
| `RedactChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. | 
| `UpdateChannelMessage` |  Allowed with Restriction | You can only edit your own sent messages\. | 
| `AssociateChannelFlow` |  Allowed |  | 
| `DisassociateChannelFlow` |  Allowed |  | 
| `GetChannelMessageStatus` |  Allowed with Restriction | You can only get message status for your own messages\. | 
|  `ListSubChannels`  | Allowed |  | 

## ChannelModerator<a name="channelmoderator"></a>

Channel moderators can perform actions only on channels for which they have the moderator role\.

**Note**  
A moderator who is an `AppInstanceAdmin` can perform actions on channels allowed by that role\.


| API name | Allowed or denied | Notes | 
| --- | --- | --- | 
| `UpdateChannel` | Allowed |  Cannot update ElasticChannelConfiguration once set | 
| `DeleteChannel` | Allowed  |  | 
| `DescribeChannel` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannel` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannelMembershipsForAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `DescribeChannelMembershipForAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `ListChannelsModeratedByAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `DescribeChannelModeratedByAppInstanceUser` | Allowed with restriction | You can also populate an [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) with another AppInstanceUser\. | 
| `CreateChannelMembership` | Allowed |  | 
| `DescribeChannelMembership` | Allowed |  | 
| `ListChannelMembership` | Allowed |  | 
| `DeleteChannelMembership` | Allowed |  | 
| `SendChannelMessage` | Allowed with restriction | You need to use [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) to create a membership for yourself first, and then call the API\. | 
| `GetChannelMessage` | Allowed |  | 
| `ListChannelMessage` | Allowed |  | 
| `DeleteChannelMessage` | Denied |  | 
| `RedactChannelMessage` | Allowed |  | 
| `UpdateChannelMessage` | Allowed with restriction | You can only update your own messages\. | 
| `CreateChannelModerator` | Allowed | You need to use [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) to create a membership for yourself first, and then call the API\. | 
| `DeleteChannelModerator` | Allowed |  | 
| `DescribeChannelModerator` | Allowed |  | 
| `ListChannelModerator` | Allowed |  | 
| `CreateChannelBan` | Allowed with restriction | The `AppInstanceUser` you are banning cannot be an `AppInstanceAdmin` or the moderator of that channel\. | 
| `DeleteChannelBan` | Allowed with restriction |  | 
| `DescribeChannelBan` | Allowed |  | 
| `ListChannelBan` | Allowed |  | 
| `UpdateChannelReadMarker` | Allowed with restriction |  For non\-elastic channels, you need to use [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) to create a membership for yourself first, and then call the API\.  Not allowed for elastic channels\.  | 
| `GetChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. Not allowed for messages in processing by channel flow unless you are the message sender\. | 
| `ListChannelMessages` |  Allowed |  | 
| `DeleteChannelMessage` |  Denied |  | 
| `RedactChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. | 
| `UpdateChannelMessage` |  Allowed with Restriction | You can only edit your own sent messages\. | 
| `AssociateChannelFlow` |  Allowed |  | 
| `DisassociateChannelFlow` |  Allowed |  | 
| `GetChannelMessageStatus` |  Allowed with Restriction | You can only get message status for your own messages\. | 
|  `ListSubChannels`  | Allowed |  | 

## Member<a name="member"></a>

An `AppInstanceUser` becomes a member of a channel if they are added to the channel via the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) API\. 

Members can perform actions only on channels to which they belong\.

**Note**  
A member who is an `AppInstanceAdmin` or `ChannelModerator` can perform actions on Channels allowed by those two roles\.


| API name | Allowed or denied | Notes | 
| --- | --- | --- | 
| `UpdateChannel` | Denied |  | 
| `DeleteChannel` | Denied |  | 
| `DescribeChannel` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannel` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannelMembershipsForAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `DescribeChannelMembershipForAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `ListChannelsModeratedByAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `DescribeChannelModeratedByAppInstanceUser` | Allowed with restriction |  You can also populate an [AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DescribeChannelModeratedByAppInstanceUser.html#API_DescribeChannelModeratedByAppInstanceUser_RequestSyntax)with another AppInstanceUser\. Not allowed for elastic channels\. | 
| `CreateChannelMembership` | Allowed with restriction | You can only add other members for an [ UNRESTRICTED ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannel.html#chime-CreateChannel-request-Mode) channel\. | 
| `DescribeChannelMembership` | Allowed |  | 
| `ListChannelMembership` | Allowed |  | 
| `DeleteChannelMembership` | Allowed |  | 
| `SendChannelMessage` | Allowed |  | 
| `GetChannelMessage` | Allowed |  | 
| `ListChannelMessage` | Allowed |  | 
| `DeleteChannelMessage` | Denied |  | 
| `RedactChannelMessage` | Allowed with restriction | You can only redact your own messages\. | 
| `UpdateChannelMessage` | Allowed with restriction | You can only update your own messages\. | 
| `CreateChannelModerator` | Denied |  | 
| `DeleteChannelModerator` | Denied |  | 
| `DescribeChannelModerator` | Denied |  | 
| `ListChannelModerator` | Denied |  | 
| `CreateChannelBan` | Denied |  | 
| `DeleteChannelBan` | Denied |  | 
| `DescribeChannelBan` | Denied |  | 
| `ListChannelBan` | Denied |  | 
| `UpdateChannelReadMarker` | Allowed with restriction |  Not allowed for elastic channels\.  | 
| `GetChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. Not allowed for messages in processing by channel flow unless you are the message sender\. | 
| `ListChannelMessages` |  Allowed |  | 
| `DeleteChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. | 
| `RedactChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. | 
| `UpdateChannelMessage` |  Allowed with Restriction | You can only edit your own sent messages\. | 
| `AssociateChannelFlow` |  Denied |  | 
| `DisassociateChannelFlow` |  Denied |  | 
| `GetChannelMessageStatus` |  Allowed with Restriction | You can only get message status for your own messages\. | 
| `Listsubchannels` | Denied |  | 

## Non\-member<a name="non-member"></a>

Non\-members are a regular `AppInstanceUser` and they cannot perform any channel related actions unless you use the [ CreateChannelMembership ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateChannelMembership.html) API to add them\.

**Note**  
A non\-member who is an `AppInstanceAdmin` or `ChannelModerator` can perform channel related actions allowed by those two roles\.


| API name | Allowed or denied | Notes | 
| --- | --- | --- | 
| `UpdateChannel` | Denied |  | 
| `DeleteChannel` | Denied |  | 
| `DescribeChannel` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannel` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannelMembershipsForAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `DescribeChannelMembershipForAppInstanceUser` | Allowed with restriction | You can also populate an [AppInstanceArn](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DescribeChannelModeratedByAppInstanceUser.html#API_DescribeChannelModeratedByAppInstanceUser_RequestSyntax) with another AppInstanceUser\. Not allowed for elastic channels\. | 
| `ListChannelsModeratedByAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `DescribeChannelModeratedByAppInstanceUser` | Allowed with restriction | You can only use your ARN as the [ AppInstanceUserArn ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_ListChannelMembershipsForAppInstanceUser.html#API_ListChannelMembershipsForAppInstanceUser_RequestSyntax) value\. | 
| `CreateChannelMembership` | Denied |  | 
| `DescribeChannelMembership` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannelMembership` | Allowed with restriction | You can only get details for public channels\. | 
| `DeleteChannelMembership` | Denied |  | 
| `SendChannelMessage` | Denied |  | 
| `GetChannelMessage` | Allowed with restriction | You can only get details for public channels\. | 
| `ListChannelMessage` | Allowed with restriction | You can only get details for public channels\. | 
| `DeleteChannelMessage` | Denied |  | 
| `RedactChannelMessage` | Denied |  | 
| `UpdateChannelMessage` | Denied |  | 
| `CreateChannelModerator` | Denied |  | 
| `DeleteChannelModerator` | Denied |  | 
| `DescribeChannelModerator` | Denied |  | 
| `ListChannelModerator` | Denied |  | 
| `CreateChannelBan` | Denied |  | 
| `DeleteChannelBan` | Denied |  | 
| `DescribeChannelBan` | Denied |  | 
| `ListChannelBan` | Denied |  | 
| `UpdateChannelReadMarker` | Denied |  | 
| `GetChannelMessage` |  Allowed with Restriction | Allowed only for sent messages\. Not allowed for messages in processing by channel flow unless you are the message sender\. | 
| `ListChannelMessages` |  Allowed with Restriction |  | 
| `DeleteChannelMessage` |  Denied | Denied | 
| `RedactChannelMessage` |  Denied |  | 
| `UpdateChannelMessage` |  Denied |  | 
| `AssociateChannelFlow` |  Denied |  | 
| `DisassociateChannelFlow` |  Denied |  | 
| `GetChannelMessageStatus` |  Allowed with Restriction | You can only get message status for your own messages\. | 