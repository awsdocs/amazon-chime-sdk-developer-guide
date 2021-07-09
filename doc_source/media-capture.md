# Creating Amazon Chime media capture pipelines<a name="media-capture"></a>

You create media capture pipelines when you need to save audio and video artifacts, events, and data messages from Amazon Chime SDK meetings\. Currently, only Amazon Chime SDK meetings support media capture pipelines, and you must use an Amazon S3 bucket as a sink for the capture\. Also, before you begin, you must integrate your client application with the AWS SDK client library\. For more information, see [ Integrating with a client library](https://docs.aws.amazon.com/chime/latest/dg/mtgs-sdk-client-lib.html)\.

**Important**  
You and your end users understand that recording Amazon Chime SDK meetings with this feature may be subject to laws or regulations regarding the recording of electronic communications\. It is your and your end users’ responsibility to comply with all applicable laws regarding the recordings, including properly notifying all participants in a recorded session that the session or communication is being recorded, and obtain their consent\. 

**Topics**
+ [Pipeline creation overview](create-pipeline.md)
+ [Creating an S3 bucket](create-s3-bucket.md)