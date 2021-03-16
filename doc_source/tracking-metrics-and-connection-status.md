# Tracking metrics and connection status<a name="tracking-metrics-and-connection-status"></a>

 The Amazon Chime SDK for JavaScript triggers the **metricsDidReceive event to provide periodic callbacks for the application to track key WebRTC metrics\. The SDK also provides callbacks for tracking a session's health and connection status:
+ *connectionHealthDidChange*
+ *connectionDidBecomePoor*
+ *connectionDidSuggestStopVideo*
+ *videoSendDidBecomeUnavailable*
+ *videoSendHealthDidChange*
+ *videoSendBandwidthDidChange*
+ *videoReceiveBandwidthDidChange*