# Setting up logging and monitoring<a name="ts-log-monitor"></a>

Logging helps you collect information such as server\-side meeting events and client\-side browser console logs\.

The Amazon Chime SDK provides server\-side meeting events that you can send to Amazon EventBridge and Amazon CloudWatch Events logs\. You can create CloudWatch metrics and insights, and use them in your dashboard for monitoring\. The [Server\-side Logging and Monitoring of Amazon Chime SDK events](http://aws.amazon.com/blogs/business-productivity/server-side-logging-and-monitoring-of-amazon-chime-sdk-events/) blog post explains how to enable the CloudWatch Metrics, Insights and Dashboard\.

The Amazon Chime SDK provides client\-side events for audio and video quality, network bandwidth, and connectivity issues\. The [Monitoring and troubleshooting with Amazon Chime SDK Meeting events](http://aws.amazon.com/blogs/business-productivity/monitoring-and-troubleshooting-with-amazon-chime-sdk-meeting-events/) blog post explains how to enable CloudWatch Metrics, Insights and Dashboard for join failures, audio quality issues, and microphone and camera setup failures\. For additional information about meeting events, see [Meeting Events](https://github.com/aws/amazon-chime-sdk-js/blob/main/guides/06_Meeting_Events.md) on Github\. 



## Options for troubleshooting metrics<a name="ts-cloudwatch-options"></a>

You have the following options for collecting troubleshooting events\.
+ Send metrics at every event 
+ Batch events every N seconds 
+ Send metrics at end of the meeting 
+ Logging level for browser console logs

## Recommended metrics<a name="ts-cloudwatch-metrics"></a>

At a minimum, you should collect and log the following metrics\.
+ SDK platform and version
+ Browser and version
+ Operating system
+ Logical cores
+ Meeting started
+ Meeting ended
+ Attendee joined
+ Attendee left
+ Attendees dropped

Additionally, depending on the issues you face, the following metrics can provide information into connectivity, bandwidth, and quality issues\. You can log ever occurence of these metrics, or just count them\. Counting can provode a summarized view of the underlying issues:
+ connectionDidSuggestStopVideo
+ connectionDidBecomeGood
+ connectionDidBecomePoor
+ videoNotReceivingEnoughData
+ Attendee join time > t seconds
+ MeetingStartFailed
+ MeetingFailed

## Enabling client\-side logging<a name="client-side-logging"></a>

You can enable `INFO`\-level browser logs by passing `LogLevel.INFO` to the `ConsoleLogger` object\.

```
const logger = new ConsoleLogger('MyLogger', LogLevel.INFO);const meetingSession = new DefaultMeetingSession(configuration,logger,deviceController); 
```

You can also use the `POSTLogger` component in the Amazon Chime SDK for JavaScript to capture browser logs in your backend, such as Amazon CloudWatch Logs\. `POSTLogger` makes `HTTP POST` requests to upload browser logs to the given URL in the [POSTLogger constructor](https://aws.github.io/amazon-chime-sdk-js/classes/postlogger.html)\. For example, the [Amazon Chime SDK serverless demo on GitHub](https://github.com/aws/amazon-chime-sdk-js/blob/main/demos/browser/app/meetingV2/meetingV2.ts#L1773) uses the `POSTLogger` to send browser logs to Amazon CloudWatch Logs for future investigation\.

## Enabling server\-side logging<a name="server-side-logging"></a>

The Amazon Chime SDK for JavaScript also calls the `eventDidReceive` observer method with key meeting events, such as `MeetingStartFailed` and `MeetingFailed`\. Meeting events often includes specific reasons for failures\. For example, say that a large group of customers experience failures\. Your web application can collect those meeting events, and then share them with us to troubleshoot the root cause\. For more information about meeting events, see the [meeting event guide on GitHub](https://aws.github.io/amazon-chime-sdk-js/modules/meetingevents.html), and the [ Monitoring and troubleshooting with Amazon Chime SDK meeting events](https://aws.amazon.com/blogs/business-productivity/monitoring-and-troubleshooting-with-amazon-chime-sdk-meeting-events/) blog post\.