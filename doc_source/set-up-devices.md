# Set up devices and join audio\-video<a name="set-up-devices"></a>

 Once the user grants permissions to devices, we choose the capture and render devices that the user selected for audio, then invoke the AudioVideoFacade\.start API to trigger the meeting join\. 

```
    await this.audioVideo.chooseAudioInputDevice(audioInputDevice);
    await this.audioVideo.chooseAudioOutputDevice(audioOutputDevice);
    this.audioVideo.start();
```