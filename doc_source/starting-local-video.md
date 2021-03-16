# Starting local video<a name="starting-local-video"></a>

 When users turn on video, you first use the *chooseVideoInputDevice* API set the chosen input device\. You then invoke the *startLocalVideoTile* API to start sending video\. You can use the *chooseVideoInputQuality* API to set the correct parameters for the outbound video stream, such as width, height, framerate, and maximum bandwidth, which controls the resolution of the stream, up to 720p\. 

```
 await this.audioVideo.chooseVideoInputDevice(device);
 this.audioVideo.startLocalVideoTile();
```