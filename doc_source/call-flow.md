# Sample call flow<a name="call-flow"></a>

This diagram shows the flow of a call through the Amazon Chime SDK PSTN Audio service and a customer’s AWS Lambda function\. In this example, the application plays a prompt to the caller, gathers dual\-tone multi frequency \(DTMF\) digits, and then connects them to an Amazon Chime SDK meeting\. 

Numbers in the diagram correspond to the numbered explanations below the diagram\.

![\[Diagram of basic call flow through the PSTN Audio service and AWS Lambda functions.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/pstn-call-flow-diagram.png)

In the diagram:

1. The Amazon Chime SDK PSTN audio service receives a call to a phone number that is provisioned in a SIP rule\.

1. The PSTN audio service fetches the associated SIP media application and invokes the associated AWS Lambda function with a `NEW_INBOUND_CALL` event \(LEG\-A\)\.

1. The AWS Lambda function returns a list of actions, including `PlayAudioAndGetDigits`, which instructs the PSTN Audio service to answer the call, play an audio file to the caller, and collect the DTMF digits entered by the caller\.

1. The PSTN Audio service answers the call, plays an audio prompt, and collects DTMF digits input by the caller\.

1. The PSTN Audio service invokes the AWS Lambda function with the DTMF digits input\. The AWS Lambda function uses the AWS SDK to create an Amazon Chime SDK meeting and a meeting attendee\. 

1. Once the AWS SDK returns a `MeetingId` and `AttendeeId`, the AWS Lambda function returns an action to join the call to the Amazon Chime SDK Meeting \(LEG\-B\)\.

1. A Real\-time Transport Protocol \(RTP\) session is established between the caller from the public switched telephone network \(PSTN\) and the Amazon Chime SDK Media service\. 

1. When the PSTN caller hangs up, the PSTN Audio service invokes the AWS Lambda function with a HANGUP event, and the AWS Lambda function deletes the attendee\. 