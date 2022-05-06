# Working with media capture artifacts<a name="artifacts"></a>

During an Amazon Chime SDK meeting, a media capture pipeline creates the following types of artifacts\. 
+ Audio
+ Video
+ Data channel messages
+ Meeting events
+ Transcription messages

The pipeline creates the artifacts in a set of folders in your Amazon S3 bucket, and you can configure the audio and video folders to limit certain types of artifacts\. The following sections explain the folder structure, how to configure folders, how to set permissions for your Amazon S3 bucket, and how to concatenate the artifact files\.

**Topics**
+ [Understanding the folder structure](#folder-structure)
+ [Configuring the audio folder](#configure-audio)
+ [Configuring the video folder](#configure-video)
+ [Understanding data messages](#data-channel)
+ [Understanding meeting events](#meeting-events)
+ [Understanding transcription messages](#transcription-messages)
+ [Setting Amazon S3 bucket permissions](#s3-permissions)
+ [Concatenating data streams](#concatenate-streams)
+ [Parsing transcripts](#parse-transcripts)

## Understanding the folder structure<a name="folder-structure"></a>



```
<S3 bucket path>/
|— audio
|— video
|— data-channel
|— meeting-events
|— transcription-messages
```

The pipeline creates new artifacts every 5 seconds\. As a result, each folder contains multiple 5\-second chunks of an artifact\.

**Note**  
If you specify a prefix when you create a media capture pipeline, the path to the folders becomes `<BucketName>/<PreFix>`\. Without a prefix, the path becomes `<BucketName>/<MeetingId>`\. You specify a prefix as part of the sink ARN path\. For example, `--sink-arn arn:aws:s3:::[bucket name]/[prefix]`

## Configuring the audio folder<a name="configure-audio"></a>

The audio folder contains 5\-second MP4 files of the mixed audio stream, meaning they contain audio from all attendees, plus the active speaker’s video\. The folder contains files for the entire meeting\. As desired, you can configure the folder to contain just the audio artifacts\. Each file name contains a <yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds> timestamp\. The timestamp is in UTC, and it marks the start time\. You can configure the folder to only contain audio artifacts\.

```
"ArtifactsConfiguration": { 
         "Audio": { 
            "MuxType": "AudioOnly"
         },
         "Content": {
            "State": "Disabled"
         },
         "Video": {
            "State": "Disabled"
         }
      }
```

## Configuring the video folder<a name="configure-video"></a>

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

## Understanding data messages<a name="data-channel"></a>

The data\-channel folder contains data messages in the \.txt format, and each message is a JSON object\. Messages are visible with all configurations options\. File names contain the <yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds> timestamp\. This example shows the data fields in a message\.

```
{
    "Timestamp": "string", 
    "Topic": "string", 
    "Data": "string", 
    "SenderAttendeeId": "string"
}
```

## Understanding meeting events<a name="meeting-events"></a>

The meeting\-events folder contains meeting events in the \.txt format, and each event is a JSON object\. Messages are visible with all configurations options\. File names contain the <yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds> timestamp\. This example shows the fields and data in a typical event file\.

```
{
    "Timestamp": "string",
    "EventType": "AttendeeJoined | AttendeeLeft | AttendeeVideoJoined | AttendeeVideoLeft | ActiveSpeaker | CaptureStarted | CaptureEnded  | AudioTrackMute | AudioTrackUnmute",
    "EventParameters": {
        # ...
    }
}
```

## Understanding transcription messages<a name="transcription-messages"></a>

The transcription\-messages folder contains transcription files in the \.txt format\. However, the folder only receives files when you enable live transcription\. For more information about enabling live transcription, see [Using Amazon Chime SDK live transcription](meeting-transcription.md)\.

The folder includes all partial and complete transcription messages, and each message is a JSON object\. File names contain the <yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds> timestamp\. You can see transcription file examples at [Delivery examples](delivery-examples.md)\.

## Setting Amazon S3 bucket permissions<a name="s3-permissions"></a>

If you haven't created an Amazon S3 bucket, make sure you create yours in the account and Region where you host meetings\. Also, make sure you grant adequate permissions to the service\. For more information about creating an Amazon S3 bucket, see [Creating an Amazon S3 bucket](create-s3-bucket.md)\.

## Concatenating data streams<a name="concatenate-streams"></a>

This example uses ffmpeg to concatenate video or audio files into a single mp4 file\. First, create a filelist\.txt file that contains all the input files\. Use this format: 

```
file 'input1.mp4'
file 'input2.mp4'
file 'input3.mp4'
```

Next, use this command to concatenate the input file:

```
ffmpeg -f concat -i filelist.txt -c copy output.mp4
```

## Parsing transcripts<a name="parse-transcripts"></a>

Use the following command to parse transcription content from a transcription message\. The command parses complete sentences from the transcript\-message\.txt files\.

```
with open('transcript-message.txt') as f:
        for line in f:
            result_json = json.loads(line)["transcript"]["results"][0]
            if result_json['isPartial'] == False:
                print(result_json["alternatives"][0]["transcript"])
```