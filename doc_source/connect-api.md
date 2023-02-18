# Establishing the connection<a name="connect-api"></a>

 After you retrieve an endpoint, you use the connect API to establish a WebSocket connection to the Amazon Chime SDK back\-end server and receive messages for an `AppInstanceUser`\. You must use AWS Signature Version 4 to sign requests\. For more information about signing a request, see [ Signing AWS Requests with Signature Version 4 ](https://docs.aws.amazon.com/general/latest/gr/Signature Version 4_signing.html)\.

**Note**  
To retrieve the endpoint, you can invoke the [ GetMessagingSessionEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_GetMessagingSessionEndpoint.html) API\. You can use the WebSocket client library of your choice to connect to the endpoint\.

**Request Syntax**

```
GET /connect
?X-Amz-Algorithm=AWS4-HMAC-SHA256
&X-Amz-Credential=AKIARALLEXAMPLE%2F20201214%2Fregion%2Fchime%2Faws4_request
&X-Amz-Date=20201214T171359Z
&X-Amz-Expires=10
&X-Amz-SignedHeaders=host
&sessionId={sessionId}
&userArn={appInstanceUserArn}
&X-Amz-Signature=db75397d79583EXAMPLE
```

**URI Request Parameters**

All URI Request Query Parameters must be URL Encoded\.

**X\-Amz\-Algorithm**

Identifies the version of AWS Signature and the algorithm that you used to calculate the signature\. The Amazon Chime SDK supports only AWS Signature Version 4 authentication, so the value of this is `AWS4-HMAC-SHA256`\.

**X\-Amz\-Credential**

In addition to your access key ID, this parameter also provides the AWS Region and service—the scope—for which the signature is valid\. This value must match the scope you use in signature calculations\. The general form for this parameter value is:

`<yourAccessKeyId>/<date>/<awsRegion>/<awsService >/aws4_request`

For example:

`AKIAIOSFODNN7EXAMPLE/20201214/us-east-1/chime/aws4_request`

**X\-Amz\-Date**

The date and time format must follow the ISO 8601 standard, and you must format it as `yyyyMMddTHHmmssZ`\. For example, you must convert **08/01/2020 15:32:41\.982\-700** to Coordinated Universal Time \(UTC\) and submit it as `20200801T083241Z`\.

**X\-Amz\-Signed\-Headers**

Lists the headers that you used to calculate the signature\. The following headers are required in the signature calculations:
+ The HTTP host header\.
+ Any x\-amz\-\* headers that you plan to add to the request\.

**Note**  
For added security, sign all the request headers that you plan to include in your request\.

**X\-Amz\-Signatures**

Provides the signature to authenticate your request\. This signature must match the signature that Amazon Chime SDK calculates\. If it doesn't, Amazon Chime SDK denies the request\. For example, `733255ef022bec3f2a8701cd61d4b371f3f28c9f19EXAMPLEd48d5193d7`\.

**X\-Amz\-Security\-Token**

Optional credential parameter if using credentials sourced from the Security Token Service\. For more information about the service, see the [https://docs\.aws\.amazon\.com/STS/latest/APIReference/](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)\.

**SessionId**

Indicates a unique Id for the WebSocket connection being established\.

**UserArn**

Indicates the identity of the `AppInstanceUser` trying to establish a connection\. The value should be the ARN of the `AppInstanceUser`\. For example, `arn:aws:chime:us%2Deast%2D1:123456789012:app%2Dinstance/694d2099%2Dcb1e%2D463e%2D9d64%2D697ff5b8950e/user/johndoe` 