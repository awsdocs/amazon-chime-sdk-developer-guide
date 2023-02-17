# Handling disconnects<a name="handle-disconnects"></a>

Websockets can disconnect due to changes in network connectivity, or when credentials expire\. After you open a WebSocket, the Amazon Chime SDK sends regular pings to the messaging client to ensure it is still connected\. If the connection closes, the client receives a WebSocket close code\. The client can try to reconnect, or not, depending on the close code\. The following tables show the close codes that the client can use to reconnect\.

For 1000 to 4000 closure codes, reconnect only for the following messages:


| Closure codes | Can reconnect | Reason | 
| --- | --- | --- | 
| 1001 | Yes | Normal closure | 
| 1006 | Yes | Abnormal closure | 
| 1011 | Yes | Internal server error | 
| 1012 | Yes | Service restart | 
| 1013 | Yes | Try again later | 
| 1014 | Yes | The server was acting as a gateway or proxy and received an invalid response from the upstream server\. This is similar to the 502 HTTP Status Code\. | 

For 4XXX codes, always reconnect *except* for the following messages:


| Closure codes | Can reconnect | Reason | 
| --- | --- | --- | 
| 4002 | No | Client initiated | 
| 4003 | No | Forbidden | 
| 4401 | No | Not authorized | 

When the application uses a close code to reconnect, the application should:

1. Call the [ GetMessagingSessionEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_GetMessagingSessionEndpoint.html) again to obtain a new base URL\. 

1. Refresh the IAM credentials if they've expired\.

1. Connect via the WebSocket\. 

If you use the amazon\-chime\-sdk\-js library, this is handled for you if you implement the the [needsRefresh\(\)](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Credentials.html#needsRefresh-property) property and the [refresh\(\)](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Credentials.html#refresh-property) method\. 