# Understanding the S3 bucket folder structure<a name="folder-structure2"></a>

The Amazon S3 buckets for media concatenation pipelines use this folder structure\.

```
S3 bucket path/
  audio
  video
  data-channel
  meeting-events
  transcription-messages
```

**Note**  
If you specify a prefix when you create a media pipeline, the path to the folders becomes *bucket name*/*prefix*\. Without a prefix, the path becomes *bucket name*/*media pipeline ID*\. You specify a prefix in the `Destination` field of the `S3BucketSinkConfiguration` object\. The concatenated file names consist of *media pipeline ID*\.mp4 for media files and *media pipeline ID*\.txt for text files\.