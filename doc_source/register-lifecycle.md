# Registering lifecycle events<a name="register-lifecycle"></a>

 The *audioVideoDidStartConnecting* callback is triggered first, indicating that we can transition to a connecting screen\. Once we establish an audio\-video session, we transition to a connected state when we receive the *audioVideoDidStart* callback\. If it fails, it triggers the *audioVideoDidStop* callback with appropriate *sessionStatus* information to describe the failure reasons\. 

```
   audioVideoDidStartConnecting(reconnecting: boolean): void {...}
   audioVideoDidStart(): void {...}
   audioVideoDidStop(sessionStatus: MeetingSessionStatus): void {...}
```