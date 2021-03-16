# Tearing down a session<a name="tearing-down"></a>

 You start tearing down a session by calling stop on the AudioVideoFacade\. If the user wants to kick everyone out of the meeting and terminate the session completely, you need an HTTP call to the Server Application – with the same authentication token – to trigger the DeleteMeeting API on the media control plane\. That ensures the meeting is terminated and all users are disconnected\. 

```
     this.audioVideo.stop();
```