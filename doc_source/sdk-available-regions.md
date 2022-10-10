# Available regions<a name="sdk-available-regions"></a>

The following tables list the features of the Amazon Chime SDK service and the AWS Regions that provide each service\.

**Note**  
Regions marked with an asterisk \(**\***\) must be enabled in your AWS account\. AWS blocks those regions by default\. For more information about enabling regions, see [Enabling a Region](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\. 

## Meeting Regions<a name="sdk-meeting-regions"></a>

Amazon Chime SDK meetings have *control regions* and *media regions*\. A control region provides the API endpoint used to create, update and delete meetings\. Media regions host the actual meetings\.

A control region can create a meeting in any media region in the same AWS partition\. However, you can only update a meeting in the control region used to create the meeting\. 

The following table lists the Regions that provde control, media, or both\.


| AWS Region | Meeting control | Meeting media | 
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

Amazon Chime SDK media pipelines have *control regions* and *data regions*\. The control region exposes the media pipeline API endpoint, and the data regions run the media pipelines\.

You can use a control region to create a media pipeline in any data region\. The media pipeline can join a meeting in any meeting media region\. 


|  AWS Region  | Control |  Data  | 
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

## Public Switched Telephone Network \(PSTN\) Regions<a name="sdk-pstn-regions"></a>

Amazon Chime SDK PSTN features have *control regions* and *media regions*\. The control regions expose the API endpoints\. The data regions connect Amazon Chime Voice Connectors with the PSTN service, and they also run SIP media applications\.


| AWS Region | Control | Media | 
| --- | --- | --- | 
| US East \(N\. Virginia\) \(us\-east\-1\) | Yes | Yes | 
| US West \(Oregon\) \(us\-west\-2\) |  | Yes | 

## Messaging Regions<a name="sdk-messaging-regions"></a>

Amazon Chime SDK messaging has *control regions* and *data regions*\. The control region exposes the messaging API endpoint, and the data region stores the messages\. If you use Amazon Kinesis to stream messaging data, Amazon S3 to store attachments, or AWS Lambda functions for channel flows, they should reside in the control region\. 


| AWS Region | Control | Data | 
| --- | --- | --- | 
| US East \(N\. Virginia\) \(us\-east\-1\) | Yes | Yes | 
| Europe \(Frankfurt\) \(eu\-central\-1\) | Yes | Yes | 