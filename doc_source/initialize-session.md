# Initialize the meeting session<a name="initialize-session"></a>

 Start by initializing the meeting session configuration object with the results of the CreateMeeting and CreateAttendee API calls passed down from the server application\. You can also pass in a logger and device controller of your choice, or use the console logger and the default device controller implementation\. Finally, create a new instance of the DefaultMeetingSession and access the key AudioVideoFacade object\. 

```
  async initializeMeetingSession(configuration: MeetingSessionConfiguration): Promise <void> {
    const logger = new ConsoleLogger('SDK', LogLevel.DEBUG);
    const deviceController = new DefaultDeviceController(logger);
    configuration.enableWebAudio = this.enableWebAudio;
    this.meetingSession = new DefaultMeetingSession(configuration, logger, deviceController);
    this.audioVideo = this.meetingSession.audioVideo;
    ...
  }
```