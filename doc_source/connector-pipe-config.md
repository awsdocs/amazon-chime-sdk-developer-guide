# Creating media live connector pipelines<a name="connector-pipe-config"></a>

The following sections list and describe the Real\-Time Messaging Protocol \(RTMP\), audio, and video settings for a media live connector pipeline\.

**RTMP settings**  
Media live connector pipelines support RTMP over a TLS/SSL connection\. The sink URL consists of the stream URL and stream key\. The URLs follow this format:

`rtmp(s)://stream-server/stream-key`

The following examples show how to connect to common streaming platforms\.
+ **Amazon Interactive Video Service \(IVS\)** – rtmps://a1b2c3d4e5f6\.global\-contribute\.live\-video\.net:443/app/*IVS\-stream\-key*
+ **YouTube** – rtmps://a\.youtube\.com/live2/*stream\-key*
+ **Twitch** – rtmps://live\.twitch\.tv/app/*primary\-stream\-key*

**Important**  
RTMPS uses encryption to help ensure that a stream is not intercepted by an unauthorized entity\. As a best practice, use RTMPS when you need additional data security\.

**Audio settings**  
Media live connector pipelines support the following audio settings:
+ **Codec** – AAC
+ **Sample rate** – 44100 Hz or 48000 Hz\. The default is 44100Hz\.
+ **Channels** – Mono or stereo\. The default is mono\.

**Video settings**  
Media live connector pipelines use the H264 encoder\. You can use HD at 1280x720 or FHD at 1920x1080\. Both resolutions use 30 frames per second, with a keyframe every two seconds\.

**Stopping media live connector pipelines**  
As a best practice for stopping media live connector pipelines, call the [DeleteMediaPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_DeleteMediaPipeline.html) API\. Ending a stream on a streaming platform such as IVS does not stop a media live connector pipeline\.