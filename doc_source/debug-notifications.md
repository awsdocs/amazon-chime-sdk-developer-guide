# Debugging push notification failures<a name="debug-notifications"></a>

The Amazon Chime SDK integrates with Amazon EventBridge in order to notify you of push message delivery failures\. To further debug failures, you can also look into the [CloudWatch metrics](https://docs.aws.amazon.com/pinpoint/latest/userguide/monitoring-metrics.html) that Amazon Pinpoint sends for failures\.

The following table lists and describes the delivery error messages\.


| Message | Description | 
| --- | --- | 
| The request processing has failed because of an unknown error, exception or failure\. | We encountered an internal error\. Please try again\. | 
| The specified resource was not found\. AppInstanceUserEndpoint will be deactivated\. | The Amazon Pinpoint application does not exist\. | 
| Too many requests sent to Amazon Pinpoint\. | Amazon Pinpoint has throttled your outgoing messages\. | 
| Unable to send messages\. Please verify IAM Permissions Policy on ServiceRoleForAmazonChimePushNotification\. | The role created for the Amazon Chime SDK does not have permission to call `mobiletargeting:SendMessages`\. Please verify the IAM policy on the role\. | 
| Unable to send messages\. Please verify IAM Trust Relationships on ServiceRoleForAmazonChimePushNotification\. | The Amazon Chime SDK does not have permission to access the role for push notificiations\.  Please verify the IAM role's trust policy contains the service principal, `messaging.chime.amazonaws.com`\. | 