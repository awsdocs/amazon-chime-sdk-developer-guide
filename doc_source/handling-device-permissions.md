# Handling device permissions<a name="handling-device-permissions"></a>

 Next, trigger the device permissions, especially for web applications\. This ensures that the user has granted the permissions needed to access the audio and video capture devices\. You can do this by using the navigator\.mediaDevices\.getUserMedia API provided by the web browsers\. This API will block your app until the user accepts or dismisses the permissions dialog box\. 

```
    this.audioVideo.setDeviceLabelTrigger(
      async (): Promise>MediaStream< => {
        this.switchToFlow('flow-need-permission');
        const stream = await navigator.mediaDevices.getUserMedia({ audio: true, video: true });
        this.switchToFlow('flow-devices');
        return stream;
      }
    );
```