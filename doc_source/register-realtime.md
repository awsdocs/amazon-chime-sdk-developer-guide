# Registering real\-time callbacks<a name="register-realtime"></a>

Once the user successfully connects to the meeting for audio and video, we can now register for real\-time callbacks to track attendee presence, mute status, signal strength change, and fatal error state\. Real\-time callbacks are received very frequently\. Be careful about processing these callbacks because they can slow performance for users\.

```
  this.audioVideo.realtimeSubscribeToAttendeeIdPresence(attendeePresenceHandler);
  this.audioVideo.realtimeSubscribeToMuteAndUnmuteLocalAudio(localAudioMuteStateHandler);
  this.audioVideo.realtimeSubscribeToSetCanUnmuteLocalAudio(canUnmuteHandler);
  this.audioVideo.realtimeSubscribeToLocalSignalStrengthChange(localSignalStrengthChangeHandler);
  this.audioVideo.realtimeSubscribeToFatalError(fatalErrorHandler);
```