# Registering for lifecycle callbacks<a name="register-callback"></a>

 The next step is to register for the lifecycle events of the AudioVideoFacade and the ContentShareController\. This drives the overall state machine of the meeting window and tells you when to transition from the connecting to the connected state, and from the connected to the disconnected state\. 

```
       this.audioVideo.addObserver(this);
       this.meetingSession.contentShare.addContentShareObserver(this);
```