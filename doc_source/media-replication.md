# Using media replication<a name="media-replication"></a>

You can use media replication to link a primary WebRTC session with multiple replica sessions to reach larger audiences\. Each WebRTC media session supports 250 connections, and you can replicate a primary session to multiple replica sessions\. Participants connected to a replica session receive only the audio and video of the presenters connected to the primary session\. They have no knowledge of the participants connected to the replicated session, which makes media replication ideal for webinars and other use cases where privacy is desired\.

The following image shows media replication between a primary session with presenters sharing audio and video, and a replica session with participants consuming the media\.

![\[Presenters sharing in a primary session.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/replication-1.png)

**Note**  
The service quota *Chime SDK Meetings \- replica meetings per primary meeting* has a default value of 4, and you can increase that limit on request\. For more information about quotas, refer to [AWS service quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *AWS General Reference*\.

**Topics**
+ [Interactive participants](#interactive-participants)
+ [Global participants](#global-participants)
+ [Session lifecycle](#session-lifecycle)

## Interactive participants<a name="interactive-participants"></a>

Participants connected to a replica session can be granted access to join the primary session\. Because everyone uses a WebRTC connection, presenters and participants don't experience transcoding delays\. When participants switch between primary and replicated sessions, they reuse their WebRTC connections, so switching is extremely fast\. That allows participants to contribute to the live conversation without missing any content\.

The following image shows a participant in a replica session using their WebRTC connection to switch to the primary session\.

![\[This is my image.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/replication-2.png)



## Global participants<a name="global-participants"></a>

You can choose the AWS Region for each WebRTC media session\. That allows you to create replica sessions in Regions closer to your participants than the primary session's Region\. When you do this, media flows from the primary session to the replica sessions across the AWS network, then from the replica session to the participant across the Internet\. When presenting to a global audience, having replica sessions near your participants can help ensure that media travels around the world on the AWS network, instead of the internet, for a better meeting experience\.

The following image shows a primary session and replicated sessions in different Regions\.

![\[This is my image.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/replication-3.png)

## Session lifecycle<a name="session-lifecycle"></a>

Creating sessions  
You use the [CreateMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateMeeting.html) or [CreateMeetingWithAttendees](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateMeetingWithAttendees.html) APIs to create WebRTC media sessions\. By default, the APIs create a primary session unless you specifically create a replica session\.  
You create a replica session by specifying the `MeetingId` of the primary session as the `PrimaryMeetingId` in the `CreateMeeting` or `CreateMeetingWithAttendees` API call\.  
If you specify the `MeetingId` of a replica session as the `PrimaryMeetingId`, the API call will fail\.

Creating attendees  
 To create the attendee credentials needed to join a WebRTC media session, you can use the [ CreateMeetingWithAttendees](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateMeetingWithAttendees.html), [CreateAttendees](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_BatchCreateAttendee.html), or [BatchCreateAttendees](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_CreateAttendee.html) APIs\.   
When creating sessions for a large number of attendees, use `CreateMeetingWithAttendees` or `BatchCreateAttendee` to minimize the number of API calls required\.

Deleting attendees  
You use the [DeleteAttendee](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_DeleteAttendee.html) API to revoke a attendee's credentials for a WebRTC media session\. If the attendee is connected to the session, they will disconnected and cannot rejoin\.  
When you use the [ DeleteMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_DeleteMeeting.html) API to delete a WebRTC media session, the API automatically deletes all attendees and you don't need to call `DeleteAttendee`\.

Switching sessions  
To allow a participant to switch from a replica session to a primary session, you must create credentials for them in the primary meeting\. Refer to *Creating attendess* earlier in this list\. Use the credentials with the `promoteToPrimaryMeeting` method in the Amazon Chime SDK client library to switch to the primary session\.  
To switch participants back to a replica session, use the `demoteToPrimaryMeeting` method in the Amazon Chime SDK client library, or use the [DeleteAttendee](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_DeleteAttendee.html) API to invalidate their primary session credentials\.  
A presenter who connects directly to a primary session can't switch to a replica session\.
For more information on switching between sessions, refer to the client library documentation:  
+ [Amazon Chime SDK for Android](https://github.com/aws/amazon-chime-sdk-android) on GitHub\.
+ [Amazon Chime SDK for iOS](https://github.com/aws/amazon-chime-sdk-ios) on GitHub\.
+ [Amazon Chime SDK client library for JavaScript](https://github.com/aws/amazon-chime-sdk-js) on GitHub\.

Deleting sessions  
You use the [ DeleteMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_meeting-chime_DeleteMeeting.html) API to delete WebRTC media sessions\.  
If you delete a primary session, the `DeleteMeeting` API automatically deletes all attached replica sessions\. So to delete all sessions, just delete the primary\.  
The service automatically deletes a primary session if no attendees connect for 5 contiguous minutes\. The service only deletes replica sessions automatically when it deletes a primary session\. That means you can create replica sessions when you create a primary session, and the replicas will be available for the duration of the primary session\.