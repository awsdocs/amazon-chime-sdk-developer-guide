# API usage metrics<a name="usage-metrics"></a>

The API usage metrics correspond to the AWS service quotas\. You can configure alarms that alert you when your usage approaches a service quota\. For more information about CloudWatch integration with service quotas, see [AWS usage metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Service-Quota-Integration.html) in the *Amazon CloudWatch User Guide*\.

The Amazon Chime SDK publishes the following API metrics in the `AWS/Usage` namespace, with the `ChimeSDK` service name\.


| Metric | Description | 
| --- | --- | 
| `CallCount` | The total number of calls made to an API in the Amazon Chime SDK\. SUM represents the total number of calls to the API during the specified period\. | 
| `ErrorCount` | The total number of errors thrown by an API in the Amazon Chime SDK\. SUM represents the total number of calls to the API during the specified period\. | 
| `ThrottleCount` | The total number of throttling errors thrown by an API in the Amazon Chime SDK\. SUM which represents the total number of calls to the API during the specified period\. | 

The Amazon Chime SDK publishes usage metrics to the `AWS/Usage` namespace with the following dimensions:


| Dimension | Description | 
| --- | --- | 
| Service | The name of the AWS service containing the resource\. For Amazon Chime SDK usage metrics, the value for this dimension is `ChimeSDK`\. | 
| Type | The type of entity being reported\. The only valid value for Amazon Chime SDK usage metrics is `API`\. | 
| Resource | The type of resource reporting the metric\. For Amazon Chime SDK usage metrics, the value for this dimension is the name of the API\. | 
| Class | The class of resource being tracked\. The only valid value for Amazon Chime SDK metrics is `None`\. | 