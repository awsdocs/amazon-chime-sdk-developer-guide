# Using Amazon Chime SDK live transcription<a name="meeting-transcription"></a>

You use Amazon Chime live transcription to generate live, user\-attributed transcripts of your meetings\. Amazon Chime live transcription integrates with the Amazon Transcribe and Amazon Transcribe Medical services to generate transcripts of Amazon Chime SDK meetings while they're in progress\.

Amazon Chime live transcription proccesses each user’s audio separately for improved accuracy in multi\-speaker scenarios\. Amazon Chime uses its active talker algorithm to select the top two active talkers, and then sends their audio to Amazon Transcribe, in separate channels, via a single stream\. Meeting participants receive user\-attributed transcriptions via Amazon Chime SDK data messages\. You can use transcriptions in a variety of ways, such as displaying subtitles, creating meeting transcripts, or using the transcriptions for content analysis\.

Live transcription uses one stream to Amazon Transcribe for the duration of the meeting transcription\. Standard Amazon Transcribe and Amazon Transcribe Medical costs apply\. For more information, refer to [https://aws\.amazon\.com/transcribe/pricing/](https://aws.amazon.com/transcribe/pricing/)\.

**Important**  
Amazon Chime live transcription is powered by Amazon Transcribe\. Use of Amazon Transcribe is subject to the [AWS Service Terms](https://aws.amazon.com/service-terms/), including the terms specific to the AWS Machine Learning and Artificial Intelligence Services\.

**Topics**
+ [System architecture](#sys-architecture)
+ [Billing and usage](#billing-and-usage)
+ [Configuring your account](configure-transcribe.md)
+ [Choosing transcription options](transcription-options.md)
+ [Starting and stopping transcription](initiate-transcription.md)
+ [Transcription parameters](#transcription-parameters)
+ [Transcription events](transcription-events.md)
+ [Transcription messages](process-msgs.md)
+ [Delivery examples](delivery-examples.md)

## System architecture<a name="sys-architecture"></a>

Amazon Chime creates real\-time meeting transcriptions, without audio leaving the AWS network, via a service\-side integration with your Amazon Transcribe or Amazon Transcribe Medical account\. For improved accuracy, users’ audio is processed separately, then mixed into the meeting\. Amazon Chime uses its active talker algorithm to select the top two active talkers, and then sends their audio to Amazon Transcribe or Amazon Transcribe Medical in separate channels via a single stream\. For reduced latency, user\-attributed transcriptions are sent directly to every meeting participant via data messages\. When using a media pipeline to capture meeting audio, the meeting’s transcription information is also captured\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/chime/latest/dg/images/transcription-architecture.png)

## Billing and usage<a name="billing-and-usage"></a>

Live transcription uses one stream to Amazon Transcribe or Amazon Transcribe Medical for the duration of the meeting transcription\. Standard Amazon Transcribe and Amazon Transcribe Medical costs [https://aws\.amazon\.com/transcribe/pricing/](https://aws.amazon.com/transcribe/pricing/) apply\. For questions about usage or billing, contact your AWS account manager\.

## Transcription parameters<a name="transcription-parameters"></a>

The Amazon Transcribe and Amazon Transcribe Medical APIs offer a number of parameters when initiating streaming transcription, such as [StartStreamTranscription](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartStreamTranscription.html) and [StartMedicalStreamTranscription](https://docs.aws.amazon.com/transcribe/latest/dg/API_streaming_StartMedicalStreamTranscription.html)\. You can use those parameters in the `StartMeetingTranscription` API unless Amazon Chime predetermines the parameter’s value\. For example, the `MediaEncoding` and `MediaSampleRateHertz` parameters are not available because Amazon Chime sets them automatically\.

Amazon Transcribe and Amazon Transcribe Medical validate the parameters, and that allows you to use new parameter values as soon as they become available\. For example, if Amazon Transcribe Medical launches support for a new language, you only need to specify the new language value in the `LanguageCode` parameter\. 