# Creating Meetings<a name="create-mtgs"></a>

The following procedure demonstrates how to create a meeting with audio and video for your server and client applications\. Before you begin, you must integrate your client application with an Amazon Chime SDK client library\. For more information, refer to [Integrating with a client library](mtgs-sdk-client-lib.md)\.

**To create a meeting with audio and video**

1. Complete the following steps from your server application:

   1. Use the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) API action in the *Amazon Chime SDK API Reference* to create a meeting\. Specify an AWS Region using the `MediaRegion` parameter\. For more information about choosing a meeting Region, refer to [Meeting Regions](sdk-available-regions.md#sdk-meeting-regions)\.

   1. Add attendees to the meeting using the [CreateAttendee](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateAttendee.html) API action or the [BatchCreateAttendee](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_BatchCreateAttendee.html) API action\. Securely transfer the meeting and attendee from your server application to the client authorized as the respective attendee\. For more information about meetings and attendees, refer to [Meeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_Meeting.html) and [Attendee](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_Attendee.html) in the *Amazon Chime SDK API Reference*\.

1. Complete the following steps from your client application:

   1. Use an Amazon Chime SDK client library to construct a `MeetingSessionConfiguration` object\. Use the meeting and attendee information from the previous steps\.

   1. Implement the `AudioVideoObserver` interface\.

   1. Create a `MeetingSession` using the `MeetingSessionConfiguration`\.

   1. Use the `AudioVideoFacade` from the `MeetingSession` to control real\-time media\.

      1. Register an instance of the `AudioVideoObserver` interface\. This lets you receive events when the meeting state changes\.

      1. Select initial devices for the audio input, audio output, and video input\.

      1. Start the audiovisual session\.

      1. Start local video capture when the user wants to share video\.

      1. To show video tiles, manage video tile events, and bind the tiles to video surfaces in the client application\.

      1. Manage other user interactions such as muting and unmuting, or starting and stopping local video capture\.

      1. To leave the meeting, stop the audiovisual session\.

   1. \(Optional\) Use the `AudioVideoFacade` from the `MeetingSession` to share media content, such as screen captures, with other clients\.

      1. Start the screen share session\. The content joins the meeting as an additional attendee\.

      1. To view the shared content, manage video tile events and bind the tiles to surfaces in the client application\.

      1. Manage other interactions, such as pausing, restarting, or stopping the content share\.

Meetings end when you run the [DeleteMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DeleteMeeting.html) API action\. Also, meetings end automatically when:
+ The meeting time exceeds 24 hours\.
+ The meeting is a [replica meeting](media-replication.md) and the primary meeting ends\.
+ In a non\-replica meeting, no attendess connect for five continuous minutes\.