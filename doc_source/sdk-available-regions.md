# Available regions<a name="sdk-available-regions"></a>

The following tables list the features of the Amazon Chime SDK service and the AWS Regions that provide each service\.

**Note**  
Regions marked with an asterisk \(**\***\) must be enabled in your AWS account\. AWS blocks those regions by default\. For more information about enabling regions, see [Enabling a Region](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\. 

## Meeting Regions<a name="sdk-meeting-regions"></a>

Amazon Chime SDK meetings have *control Regions* and *media Regions*\. A control Region provides the API endpoint used to create, update and delete meetings\. Control regions also receive and process [Meeting events](using-events.md)\. 

Media regions host the actual meetings, and clients connect to your media regions\. You specify the media region when you call the [ CreateMeeting ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_meeting-chime_CreateMeeting.html) API\.

A control region can create a meeting in any media region in the same AWS partition\. However, you can only update a meeting in the control region used to create the meeting\. 

For more information about selecting control and media regions, see [Using meeting Regions](chime-sdk-meetings-regions.md)\.

The following table lists the Regions that provde control, media, or both\.


| AWS Region | Control | Media | 
| --- | --- | --- | 
| Africa \(Cape Town\) \(af\-south\-1\)**\*** |  | Yes | 
| Asia Pacific \(Mumbai\) \(ap\-south\-1\) |  | Yes | 
| Asia Pacific \(Seoul\) \(ap\-northeast\-2\) |  | Yes | 
| Asia Pacific \(Singapore\) \(ap\-southeast\-1\) | Yes | Yes | 
| Asia Pacific \(Sydney\) \(ap\-southeast\-2\) |  | Yes | 
| Asia Pacific \(Tokyo\) \(ap\-northeast\-1\) |  | Yes | 
| Canada \(Central\) \(ca\-central\-1\) |  | Yes | 
| Europe \(Frankfurt\) \(eu\-central\-1\) | Yes | Yes | 
| Europe \(Ireland\) \(eu\-west\-1\) |  | Yes | 
| Europe \(London\) \(eu\-west\-2\) |  | Yes | 
| Europe \(Milan\) \(eu\-south\-1\)**\*** |  | Yes | 
| Europe \(Paris\) \(eu\-west\-3\) |  | Yes | 
| Europe \(Stockholm\) \(eu\-north\-1\) |  | Yes | 
| South America \(São Paulo\) \(sa\-east\-1\) |  | Yes | 
| US East \(Ohio\) \(us\-east\-2\) |  | Yes | 
| US East \(N\. Virginia\) \(us\-east\-1\) | Yes | Yes | 
| US West \(N\. California\) \(us\-west\-1\) |  | Yes | 
| US West \(Oregon\) \(us\-west\-2\) | Yes | Yes | 
|  GovCloud \(US\-East\) \(us\-gov\-east\-1\)  | Yes | Yes | 
| GovCloud \(US\-West\) \(us\-gov\-west\-1\) | Yes | Yes | 

**Note**  
To create a meeting in an AWS GovCloud \(US\) Region, you must use a control region in GovCloud\. Also, control regions in GovCloud can only make meetings in AWS GovCloud \(US\) Regions\.

## Media pipeline Regions<a name="sdk-media-pipelines"></a>

Amazon Chime SDK media pipelines have *control Regions* and *media Regions*\. A control Region provides the media pipeline API endpoint used to create and delete media pipelines\. You also use control Regions to receive and process [media pipeline events](media-pipe-events.md)\.

Media Regions run your media pipelines, and the system automatically selects the same media Region as the meeting\.

You can use a control region to create a media pipeline in any data region\. The media pipeline can join a meeting in any meeting media region\. 


| AWS Region | Control | Media | 
| --- | --- | --- | 
|  Africa \(Cape Town\) \(af\-south\-1\)**\***  |  |  Yes  | 
| Asia Pacific \(Mumbai\) \(ap\-south\-1\) |  | Yes | 
|  Asia Pacific \(Seoul\) \(ap\-northeast\-2\)  |  |  Yes  | 
|  Asia Pacific \(Singapore\) \(ap\-southeast\-1\)  | Yes |  Yes  | 
|  Asia Pacific \(Sydney\) \(ap\-southeast\-2\)  |  |  Yes  | 
|  Asia Pacific \(Tokyo\) \(ap\-northeast\-1\)  |  |  Yes  | 
|  Canada \(Central\) \(ca\-central\-1\)  |  |  Yes  | 
|  Europe \(Frankfurt\) \(eu\-central\-1\)  | Yes |  Yes  | 
| Europe \(Ireland\) \(eu\-west\-1\) |  | Yes | 
|  Europe \(London\) \(eu\-west\-2\)  |  |  Yes  | 
|  Europe \(Milan\) \(eu\-south\-1\)**\***  |  |  Yes  | 
|  Europe \(Paris\) \(eu\-west\-3\)  |  |  Yes  | 
|  Europe \(Stockholm\) \(eu\-north\-1\)  |  |  Yes  | 
|  South America \(São Paulo\) \(sa\-east\-1\)  |  |  Yes  | 
|  US East \(Ohio\) \(us\-east\-2\)  |  |  Yes  | 
| US East \(N\. Virginia\) \(us\-east\-1\) | Yes |  Yes  | 
|  US West \(N\. California\) \(us\-west\-1\)  |  |  Yes  | 
|  US West \(Oregon\) \(us\-west\-2\)  | Yes |  Yes  | 

## Voice Connector and PSTN Audio Regions<a name="sdk-pstn-regions"></a>

Amazon Chime SDK voice features have *API Regions*, *media Regions*, *PSTN Regions*, and *Alexa skill calling Regions*\. The API regions provide the API endpoints for creating and configuring SIP features\. The media Regions contain Amazon Chime SDK Voice Connectors and SIP media applications\. The PSTN Regions enable customers to connect on\-premises phone systems to the public telephone network\. Additionally, PSTN Regions support phone number provisioning and management\. The Alexa skill calling regions enable customers to place calls from devices compatible with Amazon Alexa\. For more information about Alexa skill calls, see [Using Amazon Chime SDK Alexa skill calling](alexa-calling.md)\.


| AWS Region | API | Media | PSTN | Alexa skill calling | 
| --- | --- | --- | --- | --- | 
| Asia Pacific \(Seoul\) \(ap\-northeast\-2\) | Yes | Yes |  |  | 
| Asia Pacific \(Singapore\) \(ap\-southeast\-1\) | Yes | Yes |  |  | 
| Asia Pacific \(Sydney\) \(ap\-southeast\-2\) | Yes | Yes |  |  | 
| Asia Pacific \(Tokyo\) \(ap\-northeast\-1\) | Yes | Yes |  |  | 
| Canada \(Central\) \(ca\-central\-1\) | Yes | Yes |  |  | 
| Europe \(Frankfurt\) \(eu\-central\-1\) | Yes | Yes |  |  | 
| Europe \(Ireland\) \(eu\-west\-1\) | Yes | Yes |  |  | 
| Europe \(London\) \(eu\-west\-2\) | Yes | Yes |  |  | 
| US East \(N\. Virginia\) \(us\-east\-1\) | Yes | Yes | Yes | Yes | 
| US West \(Oregon\) \(us\-west\-2\) | Yes | Yes | Yes | Yes | 

## Messaging Regions<a name="sdk-messaging-regions"></a>

Amazon Chime SDK messaging has *control regions* and *data regions*\. The control region exposes the messaging API endpoint, and the data region stores the messages\. If you use Amazon Kinesis to stream messaging data, Amazon S3 to store attachments, or AWS Lambda functions for channel flows, they should reside in the control region\. 


| AWS Region | Control | Data | 
| --- | --- | --- | 
| Europe \(Frankfurt\) \(eu\-central\-1\) | Yes | Yes | 
| US East \(N\. Virginia\) \(us\-east\-1\) | Yes | Yes | 

## <a name="console-regions"></a>