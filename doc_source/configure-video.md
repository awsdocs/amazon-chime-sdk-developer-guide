# Configuring the video folder<a name="configure-video"></a>

The video folder contains 5\-second MP4 files that contain video streams, plus content share streams if they’re specified in the API request\. Each file name contain a <yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds>\-<attendeeID> timestamp with an attendee ID\. The content share video chunk is appended as <yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds>\-<attendeeID>\#content\.mp4\. You can configure the folder to only contain video artifacts\.

```
"ArtifactsConfiguration": { 
         "Audio": { 
            "MuxType": "AudioOnly"
         },
         "Content": {
            "State": "Disabled"
         },
         "Video": {
            "MuxType": "VideoOnly"
            "State": "Enabled"
         }
      }
```