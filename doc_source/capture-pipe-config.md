# Creating media capture pipelines<a name="capture-pipe-config"></a>

Media capture pipelines capture audio, video, and content share streams, plus meeting events and data messages\. All media capture pipelines save their data to [Amazon Simple Storage Service](https://aws.amazon.com/s3/) \(S3\) bucket that you create\. You can create one media capture pipeline per Amazon Chime SDK meeting\.

The following sections explain how to create a media capture pipeline\. Follow them in the order listed\.

**Topics**
+ [Creating an Amazon S3 bucket](create-s3-bucket.md)
+ [Enabling server\-side encryption for an Amazon S3 bucket](sse-kms.md)
+ [Working with media capture artifacts](artifacts.md)
+ [Configuring the audio folder](configure-audio.md)
+ [Configuring the video folder](configure-video.md)
+ [Understanding messages in the data\-channel folder](data-channel.md)
+ [Understanding the S3 bucket folder structure](folder-structure.md)
+ [Understanding meeting event files](meeting-events.md)
+ [Understanding transcription files](transcription-messages.md)
+ [Concatenating data streams](concatenate-streams.md)