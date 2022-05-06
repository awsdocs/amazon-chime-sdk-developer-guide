# Recording audio tracks<a name="record-legs"></a>

You can record just the incoming or outgoing tracks of call, or both tracks of a call\.

This image shows a typical one\-legged, or non\-bridged, incoming call\. 

![\[An incoming call that only communicates with a SIP media application.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/call-record-sma-one-leg.png)

The call only has one leg with a `callID` of **call\-id\-1**\. The `INCOMING` audio track is the audio from the caller to the SIP media application\. The `OUTGOING` audio track is the audio from the SIP media application to the caller\. Your SIP media application specifies the `CallId` of the call that you want record\. To record the participant who placed the call, you specify `INCOMING`\. To record the participant who responds to a call, you specify `OUTGOING`\. To record both participants, specify `BOTH`\.

This image shows a typical bridged call with two participants\.

![\[An incoming call that communicates with a SIP media application and a second participant.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/call-record-sma-bridged.png)

In this example, the call has two call legs, **call\-id\-1** and **call\-id\-2**, and **call\-id\-1** is bridged to **call\-id\-2**\. This creates four audio tracks, the incoming and outgoing audio streams for both call IDs\. You can specify which of the call IDs and audio tracks to record\. For example, if you want to record the audio track from the called participant, you record the `INCOMING` audio track by specifying **call\-id\-2** as the `CallId` and `INCOMING` as the track\.

If you want to record everything that the caller hears, you record the `OUTGOING` audio track by specifying **call\-id\-1** as the `CallId` and `OUTGOING` as the track\. If you want to record all of the audio that the `Caller` said and heard, you record `BOTH` audio tracks by specifying `call-id-1` as the `CallId` and `BOTH` as the track\.