# Using call detail records<a name="attributes"></a>

Amazon Chime administrators can configure Amazon Chime Voice Connectors to store *call detail records* \(CDRs\)\. For more information about configuring Amazon Chime Voice Connectors to store CDRs, see [Managing global settings in Amazon Chime](https://docs.aws.amazon.com/chime/latest/ag/manage-global.html), in the *Amazon Chime administrator's guide*\.

Shortly after each call, the Amazon Chime Voice Connector stores the CDRs as objects in your S3 bucket\. You can import the CDRs into a VoIP billing system\. 

CDR objects must not contain newline characters or white spaces\. The following table lists the attributes of a CDR, and shows the proper formatting:


|  Value  |  Description  | 
| --- | --- | 
|  `"AwsAccountId":"AWS-account-ID",`  |  AWS account ID  | 
|  `"TransactionId":"transaction-ID", `  |  Amazon Chime Voice Connector transaction ID UUID  | 
|  `"CallId":"SIP-media-application-call-ID",`  |  Customer facing SIP call ID  | 
|  `"VoiceConnectorId":"voice-connector-ID",`  |  Amazon Chime Voice Connector ID UUID  | 
|  `"Status":"status",`  |  Status of the call  | 
|  `"StatusMessage":"status-message",`  |  Status message of the call  | 
|  `"SipAuthUser":"sip-auth-user",`  |  SIP authentication name  | 
|  `"BillableDurationSeconds":"billable-duration-in-seconds",`  |  Billable duration of the call in seconds  | 
|  `"BillableDurationMinutes":"billable-duration-in-minutes",`  |  Billable duration of the call in minutes  | 
|  `"SchemaVersion":"schema-version",`  |  The CDR schema version  | 
|  `"SourcePhoneNumber":"12075550155",`  |  E\.164 origination phone number  | 
|  `"SourceCountry":"source-country",`  |  Country of origination phone number  | 
|  `"DestinationPhoneNumber":"13605551214",`  |  E\.164 destination phone number  | 
|  `"DestinationCountry":"destination-country",`  |  Country of destination phone number  | 
|  `"UsageType":"usage-type",`  |  Usage details of the line item in the Price List API  | 
|  `"ServiceCode":"service-code", `  |  The code of the service in the Price List API  | 
|  `"Direction":"direction",`  |  Direction of the call, `Outbound` or `Inbound`  | 
|  `"StartTimeEpochSeconds":"start-time-epochseconds",`  |  Indicates the call start time in epoch/Unix timestamp format  | 
|  `"EndTimeEpochSeconds":"end-time-epochseconds",`  |  Indicates the call end time in epoch/Unix timestamp format  | 
|  `"Region":"AWS-region"}`  |  AWS region for the Amazon Chime Voice Connector  | 
|  `"Streaming":{"true\|false"}`  |  Indicates whether the Streaming audio option was enabled\.  | 