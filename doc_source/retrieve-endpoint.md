# Retrieving the endpoint<a name="retrieve-endpoint"></a>

The following steps explain how to retrieve the endpoint used in a WebSocket connection\.

1. Use the [ GetMessagingSessionEndpoint ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_GetMessagingSessionEndpoint.html) API to retrieve the WebSocket endpoint\. 

1. Use the URL returned by the [ GetMessagingSessionEndpoint ](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_GetMessagingSessionEndpoint.html) API to construct a Signature Version 4 Signed WebSocket URL\. If you need help doing that, you can follow directions in the [Establishing the connection](connect-api.md)\.