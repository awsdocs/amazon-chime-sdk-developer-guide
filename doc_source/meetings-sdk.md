# Using the Amazon Chime SDK<a name="meetings-sdk"></a>

You use the Amazon Chime SDK to build real\-time media applications that can send and receive audio and video and allow content sharing\. The Amazon Chime SDK works independently of any Amazon Chime administrator accounts, and it does not affect meetings hosted on Amazon Chime\. Instead, the Amazon Chime SDK provides builder tools that you use to build your own meeting applications\.

**Topics**
+ [Amazon Chime SDK prerequisites](#mtg-prereqs)
+ [Amazon Chime SDK concepts](#mtg-glossary)
+ [Amazon Chime SDK architecture](#mtg-arch)
+ [Amazon Chime SDK service quotas](#mtg-limits)
+ [Amazon Chime SDK system requirements](#mtg-browsers)
+ [Available regions](sdk-available-regions.md)
+ [Integrating with a client library](mtgs-sdk-client-lib.md)
+ [SIP integration using an Amazon Chime SDK Voice Connector](mtgs-sdk-cvc.md)
+ [Amazon Chime SDK event notifications](mtgs-sdk-notifications.md)

## Amazon Chime SDK prerequisites<a name="mtg-prereqs"></a>

Using the Amazon Chime SDK requires the following:
+ The ability to program\.
+ An AWS account\.
+ An IAM role with a policy that grants permission to access Amazon Chime API actions used by the Amazon Chime SDK, such as the AWS managed **AmazonChimeSDK** policy\. For more information, see [How Amazon Chime works with IAM](https://docs.aws.amazon.com/chime-sdk/latest/ag/security_iam_service-with-iam.html) and [Allow users to access Amazon Chime SDK actions](https://docs.aws.amazon.com/chime-sdk/latest/ag/security_iam_id-based-policy-examples.html#security_iam_id-based-policy-examples-chime-sdk) in the *Amazon Chime SDK Administrator Guide*\.
+ For the majority of use cases, you also need the following:
  + A **server application** – Manages meeting and attendee resources, and serves those resources to the client application\. The server application is created in the AWS account and must have access to the IAM role mentioned previously\.
  + A **client application** – Receives meeting and attendee information from the server application, and uses that information to make media connections\.

## Amazon Chime SDK concepts<a name="mtg-glossary"></a>

The following terminology and concepts are central to understanding how to use the Amazon Chime SDK\.

**meeting**  
An ephemeral resource identified by a unique `MeetingId`\. The `MeetingId` is placed onto a group of media services that host the active meeting\.

**media service group**  
The group of media services that hosts an active meeting\.

**media placement**  
A set of regionalized URLs that represents a media service group\. Attendees connect to the media service group with their clients to send and receive real\-time audio and video, and share their screens\.

**attendee**  
A meeting participant that is identified by a unique `AttendeeId`\. Attendees may freely join and leave meetings using a client application built with an Amazon Chime SDK client library\.

**join token**  
A unique token assigned to each attendee\. Attendees use the join token to authenticate with the media service group\.

## Amazon Chime SDK architecture<a name="mtg-arch"></a>

The following list describes how the different components of the Amazon Chime SDK architecture work together to support meetings and attendees, audio, video, and content sharing\.

**Meetings and attendees**  
When the server application creates an Amazon Chime SDK meeting, the meeting is assigned to a region\-specific media service\. The hosts in the service are responsible for securely transferring real\-time media between attendee clients\. Each created attendee is assigned a unique join token, an opaque secret key that your server application must securely transfer to the client authorized to join the meeting on behalf of an attendee\. Each client uses a join token to authenticate with the media service group\. Clients use a combination of secure WebSockets and Datagram Transport Layer Security \(DTLS\) to securely signal the media service group, and to send and receive media to and from other attendees through the media service group\.

**Audio**  
The media service mixes audio together from each attendee and sends the mix to each recipient, after subtracting their own audio from the mix\. The Amazon Chime SDKs sample audio at the highest rate supported by the device and browser, up to a maximum of 48kHz\. We use the Opus codec to encode audio, with a default bitrate of 32kbps, which can be increased to up to 128kbps stereo and 64kbps mono\.

**Video**  
The media service acts as a Selective Forwarding Unit \(SFU\) using a publish and subscribe model\. Each attendee can publish one video source, up to a total of 25 simultaneous videos per meeting\. The Amazon Chime SDK client library for JavaScript supports video resolutions up to 1280x720 at 30 frames per second without simulcast, and 15 frames per second with simulcast\. The Amazon Chime SDK client libraries for [iOS](sdk-for-ios.md), [Android](sdk-for-android.md), and [Windows](client-lib-windows.md) support video resolutions up to 1280x720 and 15 frames per second, however the actual framerate and resolution is automatically managed by the Amazon Chime SDK\.  
When active, video simulcast sends each video stream in two different resolutions and bitrates\. Clients with bandwidth constraints automatically subscribe to the lower bitrate stream\. Video encoding and decoding uses hardware acceleration where available to improve performance\.

**Data messages**  
In addition to audio and video content, meeting attendees can send each other real\-time data messages of up to 2 KB each\. You can use messages to implement custom meeting features such as whiteboarding, chat, real\-time emoji reactions, and application\-specific floor control signaling\.

**Content sharing**  
The client application can share audio and video content, such as screen captures or media files\. Content sharing supports pre\-recorded content video up to 1280x720 at 15 frames per second, and audio up to 48kHz at 64kbps\. Screen capture for content sharing is supported up to 15 frames per second, but may be limited by the capabilities of the device and browser\.

## Amazon Chime SDK service quotas<a name="mtg-limits"></a>

**Note**  
Service quotas are per API endpoint\. When requesting a service quota increase, be sure to request the increase on all the API endpoints that your application uses\.

This table that lists the resources and quotas available for Amazon Chime SDK meetings\.


| Resource | Quota | Adjustable | 
| --- | --- | --- | 
| Active Meetings |  250  |  Yes  | 
|  Attendees per meeting  |  250  |  No  | 
|  Audio streams per meeting  |  250  |  No  | 
|  Video streams published per meeting  | 25 |  Yes, up to 250   | 
|  Video streams subscribed per attendee  |  25  |  No  | 
|  Content shares per meeting  |  2  |  No  | 
| Replica meetings per primary meeting | 4 | Yes, up to 40 | 
| Active media capture pipelines per meeting | 1 | No | 
| Active media capture pipelines per account | 100 for us\-east\-1 endpoints, and 10 for other endpoints | Yes | 
|  API Rate  |  10 requests per second \(RPS\) with a burst of 20 RPS\.  |  Yes, but indirectly API rate limits are increased when you increase the Active Meetings quota\.  | 

## Amazon Chime SDK system requirements<a name="mtg-browsers"></a>

The following system requirements apply to applications created with the Amazon Chime SDK\.

**Supported browsers, Amazon Chime SDK client library for JavaScript**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/meetings-sdk.html)

**Amazon Chime SDK client library for iOS**
+ iOS version 13 and later

**Amazon Chime SDK client library for Android**
+ Android OS version 5 and later, ARM and ARM64 architecture