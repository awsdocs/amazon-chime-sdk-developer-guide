# Messaging quotas<a name="messaging-quotas"></a>

The Amazon Chime SDK messaging enforces the following quotas\.


| Resource | Limit | Eligible for increase | 
| --- | --- | --- | 
| App instances per AWS account | 100 | Yes | 
| Users per app instance | 100,000 | Yes | 
| Admins per app instance | 100 | Yes | 
| Channels per app instance | 10,000,000 | Yes | 
| Memberships per channel | 10,000 | Yes**\*** | 
| Moderators per channel | 1,000 | Yes | 
| Max concurrent connections per user \(Amazon Chime SDK messaging only, does not apply to meetings\) | 10 | Yes | 
| Channel flows per App Instance | 100 | Yes | 
| Channel processors in a channel flow | 1 | Yes | 
| Endpoints per app instance user | 10 | Yes | 
| Number of `CHANNEL_DETAILS` events | 50 | Yes | 
| Number of `ChannelMessages` in `CHANNEL_DETAILS` | 20 | Yes | 
| Number of `ChannelMemberships` in `CHANNEL_DETAILS` | 30 | Yes | 

**\***In US\-EAST \(N\. Virginia\)