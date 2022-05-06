# Using call recording<a name="sip-apps-call-record"></a>

The call recording actions for SIP media applications enable you to build call recording and post\-call transcription solutions for a variety of uses\. For example, you can record customer\-care calls and use them for training\.

You use the call recording actions in concert with your SIP media applications\. You can also use the actions on\-demand or in response to a SIP event\. 
+ To start on\-demand recording of a call in your SIP media application, you use the [UpdateSipMediaApplication](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_UpdateSipMediaApplication.html) API to invoke your application and return the [StartCallRecording](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_StartCallRecording.html) action\. 
+ To start call recording in response to a SIP event, you return the `StartCallRecording` action in your application\. 

You can pause and resume an in\-progress recording\. To pause, use the [PauseCallRecording](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_PauseCallRecording.html) action\. To resume, use the `ResumeCallRecording` action\. Each time you pause or resume a recording, the action captures a tone that indicates the pause or resumption\. When you pause, the action records silence, which Amazon Chime SDK uses to track the length of the pause and include the pauses in your bill\. You can pause and resume recording as often as needed\. 

To stop call recording, you return the [StopCallRecording](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_StopCallRecording.html) action\. However, call recordings automatically stop when the call stops, and in that case you don’t need to explicitly return the `StopCallRecording` action\. You can only start and stop recording once for an individual call leg\.

Amazon Chime SDK delivers call recordings to an Amazon S3 bucket that you select\. The Amazon S3 bucket must belong to your AWS account\. Once a call stops, the SIP media application delivers the recording to the folder specified in the `Destination` parameter of the [StartCallRecording](start-call-recording.md) action\. Amazon Chime records calls in an open WAV format\. Calls that record incoming and outgoing tracks use stereo mode, with the incoming track in the left channel and the outgoing track on the right channel\. If you record only the incoming or outgoing track, the system uses mono mode\.

**Note**  
Recordings made using this feature may be subject to laws or regulations regarding the recording of electronic communications\. It is your and your end users’ responsibility to comply with all applicable laws regarding the recording, including properly notifying all participants in a recorded session or communication that the session or communication is being recorded, and obtaining their consent\.

## Billing for call recording<a name="call-billing"></a>

Amazon Chime SDK bills you per minute for the time that call recording is enabled for a call leg, and that time includes all pauses\. You are billed for the call recording usage once the call recording is delivered to your Amazon S3 bucket\.