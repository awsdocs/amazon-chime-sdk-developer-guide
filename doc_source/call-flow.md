# Sample call flow<a name="call-flow"></a>

This diagram shows the flow of a call through a SIP media application and its associated Lambda function\. In this example, the application plays a prompt to the caller, gathers dual\-tone multi frequency \(DTMF\) digits, and then connects them to an Amazon Chime SDK meeting\.

Numbers in the diagram correspond to the numbered explanations below the diagram\.

![\[Diagram of basic call flow through a SIP media application and Lambda functions.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SMA Images-CS-2021-05-14-Call Flow.png)

In the diagram:

1. User initiates call to the SIP media application \(LEG\-A\)\.

1. The `NEW_INBOUND_CALL` event invokes your Lambda function\. The Lambda function returns the `PlayAudioAndGetDigits` action\.

1. The SIP media application answers the call, plays an audio prompt, and collects DTMF digits input by the caller\.

1. The SIP media application invokes the Lambda function with the DTMF digits input\. The Lambda function uses the AWS SDK to create an Amazon Chime SDK meeting and a meeting attendee\.

1. Once the AWS SDK returns a `MeetingId` and `AttendeeId`, the Lambda function returns an action to join the call to the Amazon Chime SDK Meeting \(LEG\-B\)\.

1. A Real\-time Transport Protocol \(RTP\) session is established between the caller from the public switched telephone network \(PSTN\) and the Amazon Chime Media Service\.

1. When the PSTN caller hangs up, the SIP media application invokes the Lambda function with a `HANGUP` event, and the Lambda function deletes the attendee\.