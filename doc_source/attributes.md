# Using call detail records<a name="attributes"></a>

Amazon Chime SDK administrators can configure Amazon Chime SDK Voice Connectors to store *call detail records* \(CDRs\)\. For more information about configuring Amazon Chime SDK Voice Connectors to store CDRs, see [Managing global settings in Amazon Chime SDK](https://docs.aws.amazon.com/chime-sdk/latest/ag/manage-global.html) in the *Amazon Chime SDK Administration Guide*\.

Once you enable CDRs, after each call the SIP media application sends the records to a folder named **Amazon\-Chime\-SMADRs** in your S3 bucket\.

The following table lists the attributes of a CDR, and shows their proper formatting\. The records contain all the fields listed here for all calls\.


|  Value  |  Description  | 
| --- | --- | 
|  `"AwsAccountId":"AWS-account-ID"`  |  The AWS account ID associated with the SIP media application that initiated the PSTN usage  | 
|  `"TransactionId":"transaction-ID" `  |  The transaction ID of the call  | 
|  `"CallId":"SIP-media-application-call-ID"`  |  The call ID of the participant for the associated usage  | 
|  `"VoiceConnectorId":"voice-connector-ID"`  |  Amazon Chime SDK Voice Connector ID UUID  | 
|  `"Status":"status"`  |  Status of the call \(Completed, Failed\)  | 
|  `"BillableDurationSeconds":"billable-duration-in-seconds"`  |  Billable duration of the call in seconds  | 
|  `"SchemaVersion":"schema-version"`  |  The CDR schema version  | 
|  `"SourcePhoneNumber":"12075550155"`  |  E\.164 origination phone number  | 
|  `"DestinationPhoneNumber":"13605551214"`  |  E\.164 destination phone number  | 
|  `"UsageType":"usage-type"`  |  Usage details of the line item in the Price List API  | 
|  `"ServiceCode":"service-code", `  |  The code of the service in the Price List API  | 
|  `"Direction":"direction"`  |  Direction of the call, `Outbound` or `Inbound`  | 
|  `"TimeStampEpochSeconds":"start-time-epochseconds",`  |  The timestamp of the record in epoch/Unix timestamp format  | 
|  `"Region":"AWS-region"}`  |  AWS Region for the Amazon Chime SDK Voice Connector  | 
|  `"SipRuleId":"sip-rule-id"`  |  The ID of the sip rule that is triggered when a call reaches the PSTN Audio service  | 
|  `"SipApplicationId":"sip-application-id"`  |  The ID of the SIP application that handles a call  | 
|  `"CallLegTriggerType":"trigger-type"`  |  The type of event that triggered a call  | 
|  `"BillableVoiceFocusSeconds":"billable-voice-focus-in-seconds"`  |  The billable amount of Voice Focus usage, in seconds  | 