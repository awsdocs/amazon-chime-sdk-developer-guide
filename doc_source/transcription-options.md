# Choosing transcription options<a name="transcription-options"></a>

When you use Amazon Chime SDK live transcription, you use [Amazon Transcribe](https://aws.amazon.com/transcribe/) or [Amazon Transcribe Medical](https://aws.amazon.com/transcribe/medical/) in your AWS account\. You have access to all the [streaming languages supported by Amazon Transcribe](https://docs.aws.amazon.com/transcribe/latest/dg/what-is-transcribe.html), plus features such as [custom vocabularies](https://docs.aws.amazon.com/transcribe/latest/dg/how-vocabulary.html) and [vocabulary filters](https://docs.aws.amazon.com/transcribe/latest/dg/filter-unwanted-words.html)\. When using Amazon Transcribe Medical, you can choose a medical specialty, conversation type, and optionally provide any custom vocabulary\. Standard Amazon Transcribe and Amazon Transcribe Medical costs apply\.

The process of choosing transcription options follows these steps\. 

## Step 1: Choosing a transcription service<a name="choose-service"></a>

You need to decide which transcription service to use, [Amazon Transcribe](https://aws.amazon.com/transcribe/) or [Amazon Transcribe Medical](https://aws.amazon.com/transcribe/medical/)\. 

If your use case requires medical speech to text capabilities, you probably want to use Amazon Transcribe Medical\. For all other use cases, you probably want to use Amazon Transcribe\.

You specify which transcription service to use when you call the `StartMeetingTranscription` API:
+ To use Amazon Transcribe, specify a `TranscriptionConfiguration` with `EngineTranscribeSettings`\. 
+ To use Amazon Transcribe Medical, specify a `TranscriptionConfiguration` with `EngineTranscribeMedicalSettings`\.

## Step 2: Choosing a transcription Region<a name="choose-region"></a>

You need to choose an AWS Region for the transcription service\. For information about the available AWS Regions for Amazon Transcribe and Amazon Transcribe Medical, refer to the [AWS Regional Services](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) table\.

 In general, the lowest latency between a meeting’s media Region and transcription Region provides the best user experience\. For the lowest latency, use the same Region for media and transcription whenever possible\. However, you may have other factors to consider in selecting a Region, such as regulatory requirements or the Regions where you have configured Amazon Transcribe or Amazon Transcribe Medical\.

Amazon Transcribe and Amazon Transcribe Medical features, such as custom vocabularies or vocabulary filters, are Region specific\. If you configure any of those features, you should do so identically in all of the AWS Regions in which you intend to use live transcription\. Alternately, you can use the same Amazon Transcribe Region for all meetings\.

You can specify the Region that the transcription service uses\. You do that by adding the region name to the `Region` field of the transcription engine settings when you call the `StartMeetingTranscription` API\. If you do not specify a Region, the Amazon Chime SDK attempts to use the transcription service in the meeting’s media region\. To have the Amazon Chime SDK select the Region for the transcription service for you, specify `auto` in the `Region` field\. When you do that, Amazon Chime selects the transcription service Region based the meeting’s media Region as described in the tables below\. For more information about the `StartMeetingTranscription` API, refer to [Starting and stopping transcription](initiate-transcription.md) in this guide\.

**Note**  
The transcription region selected by the Amazon Chime SDK is subject to change as AWS, Amazon Chime SDK, Amazon Transcribe and Amazon Transcribe Medical make more regions available\.

**Automatic region selection for Amazon Transcribe**  



|  Amazon Chime SDK Media Region  |  Region code  |  Transcription Region  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  | us\-east\-2  | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | us\-east\-1  | 
|  US West \(N\. California\)  |  us\-west\-1  | us\-west\-2 | 
|  US West \(Oregon\)  |  us\-west\-2  | us\-west\-2  | 
|  Africa \(Cape Town\)**\***  |  af\-south\-1  | eu\-west\-2  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  | eu\-west\-2 | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  | ap\-northeast\-2 | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  | ap\-northeast\-1 | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  | ap\-southeast\-2 | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1  | ap\-northeast\-1 | 
|  Canada \(Central\)  |  ca\-central\-1  | ca\-central\-1 | 
|  Europe \(Frankfurt\)   |  eu\-central\-1  | eu\-central\-1  | 
|  Europe \(Ireland\)  |  eu\-west\-1  | eu\-west\-1 | 
|  Europe \(London\)  |  eu\-west\-2  | eu\-west\-2  | 
|  Europe \(Milan\)**\***  |  eu\-south\-1  | eu\-central\-1  | 
|  Europe \(Paris\)  |  eu\-west\-3  | eu\-central\-1  | 
|  Europe \(Stockholm\)  |  eu\-north\-1  | eu\-central\-1 | 
|  South America \(São Paulo\)  |  sa\-east\-1  | sa\-east\-1 | 
|  GovCloud \(US\-East\)  |  us\-gov\-east\-1  |  us\-gov\-west\-1  | 
|  GovCloud \(US\-West\)  |  us\-gov\-west\-1  |  us\-gov\-west\-1  | 

**Automatic region selection for Amazon Transcribe Medical**  



|  Amazon Chime SDK Media Region  |  Region code  |  Transcription Region  | 
| --- | --- | --- | 
|  US East \(Ohio\)  |  us\-east\-2  | us\-east\-2 | 
|  US East \(N\. Virginia\)  |  us\-east\-1  | us\-east\-1 | 
|  US West \(N\. California\)  |  us\-west\-1  | us\-west\-2 | 
|  US West \(Oregon\)  |  us\-west\-2  | us\-west\-2 | 
|  Africa \(Cape Town\)**\***  |  af\-south\-1  |  eu\-west\-1  | 
|  Asia Pacific \(Mumbai\)  |  ap\-south\-1  | eu\-west\-1  | 
|  Asia Pacific \(Seoul\)  |  ap\-northeast\-2  | us\-west\-2 | 
|  Asia Pacific \(Singapore\)  |  ap\-southeast\-1  | ap\-southeast\-2 | 
|  Asia Pacific \(Sydney\)  |  ap\-southeast\-2  | ap\-southeast\-2 | 
|  Asia Pacific \(Tokyo\)  |  ap\-northeast\-1  | us\-west\-2 | 
|  Canada \(Central\)  |  ca\-central\-1  | ca\-central\-1 | 
|  Europe \(Frankfurt\)   |  eu\-central\-1  | eu\-west\-1 | 
|  Europe \(Ireland\)  |  eu\-west\-1  | eu\-west\-1 | 
|  Europe \(London\)  |  eu\-west\-2  | us\-east\-1 | 
|  Europe \(Milan\)**\***  |  eu\-south\-1  | eu\-west\-1 | 
|  Europe \(Paris\)  |  eu\-west\-3  | eu\-west\-1 | 
|  Europe \(Stockholm\)  |  eu\-north\-1  | eu\-west\-1 | 
|  South America \(São Paulo\)  |  sa\-east\-1  | us\-east\-1 | 

**Note**  
To use live transcription in Regions marked with an asterisk \(**\***\), you must first enable the Region in your AWS account\. For more information, refer to [Enabling a Region](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the AWS General Reference\.

For more information about the regions and endpoints for each service, refer to:
+ [Amazon Chime SDK media Regions](https://docs.aws.amazon.com/chime-sdk/latest/dg/chime-sdk-meetings-regions.html)
+ [Amazon Transcribe endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/transcribe.html#transcribe_region)
+ [Amazon Transcribe Medical endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/transcribe-medical.html)

## Step 3: Review service quotas<a name="transcribe-quotas"></a>

Each Amazon Chime SDK meeting with live transcription requires exactly one HTTP/2 stream to Amazon Transcribe or Amazon Transcribe Medical\. Both services have regional service quotas for the number of concurrent HTTP/2 streams, and for start\-stream transactions per second\. For more information about the quotas, refer to [Guidelines and quotas](https://docs.aws.amazon.com/transcribe/latest/dg/limits-guidelines.html) in the *Amazon Transcribe Developer Guide*\. For information about quota increases, refer to Service Quotas in the AWS console\.