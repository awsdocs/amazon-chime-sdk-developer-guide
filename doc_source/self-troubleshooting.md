# Self troubleshooting<a name="self-troubleshooting"></a>

The sections in this topic explain several ways to self\-troubleshoot Amazon Chime SDK meetings\.

**Topics**
+ [Checking FAQs and known issues](#check-faqs)
+ [Verifying network access](#net-acess)

## Checking FAQs and known issues<a name="check-faqs"></a>

Check these FAQs and lists of known issues on GitHub for troubleshooting and debugging adivce\.
+ [Amazon Chime SDK – JavaScript \- Meetings](https://github.com/aws/amazon-chime-sdk-js/blob/main/guides/07_FAQs.md#meetings)
+ [Amazon Chime SDK – JavaScript \- Media](https://github.com/aws/amazon-chime-sdk-js/blob/main/guides/07_FAQs.md#media)
+ [Amazon Chime SDK – JavaScript \- Networking](https://github.com/aws/amazon-chime-sdk-js/blob/main/guides/07_FAQs.md#networking)
+ [Amazon Chime SDK – \- Audio and Video](https://github.com/aws/amazon-chime-sdk-js/blob/main/guides/07_FAQs.md#audio-and-video)

## Verifying network access<a name="net-acess"></a>

Enterprises often have have network firewalls that restrict access to specific ports, or connections to IP address ranges out of their network\. The following sections explain some of the ways you can verify network access\.

**Topics**
+ [Validating AWS SDK and Amazon Chime SDK subnets and ports](#subnets-ports)
+ [Using demo apps to reproduce issues](#repro-on-demo-apps)
+ [Using the Meeting Readiness Checker](#ready-checker)

### Validating AWS SDK and Amazon Chime SDK subnets and ports<a name="subnets-ports"></a>

Applications that use the Amazon Chime SDK utilize two tiers, server and client\. The server tier uses the AWS SDK and has server\-side meeting handlers\. The client tier uses client SDKs\.

The AWS SDK is used to call server APIs such as [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_meeting-chime_CreateMeeting.html)\. Such APIs connect to the AWS global service endpoints in the `us-east-1`, `us-west-2`, `ap-southeast-1`, `eu-central-1`, `us-gov-east-1`, and `us-gov-west-1` Regions\. The [AWS IP address ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) page in the *AWS General Reference* lists the IP address ranges for each Region\. For information about service endpoints and quotas, see the [Amazon Chime SDK endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/chime-sdk.html)\.

Client SDKs, such as the Amazon Chime SDK for JavaScript, connect to service endpoints in the `*.chime.aws` domain\. 

Use the following validations to ensure that you have network permissions:
+ Run the [Amazon Chime SDK Meeting Readiness Checker](https://github.com/aws/amazon-chime-sdk-js#meeting-readiness-checker) on GitHub to verify that you can reach your network and ports\.
+ Verify you can resolve \*\.chime\.aws domain from your network or your end user’s network\. 
+ Ensure that your firewall allows connections to the AWS IP range via TCP Port 443 for control commands and UDP Port 3478 for media\. 

### Using demo apps to reproduce issues<a name="repro-on-demo-apps"></a>

As a best practice, start the debugging process by trying to reproduce your issue in one of demo apps\. That enables the service team to locate where the issue might be\. If you can't reproduce the issue with a demo app, you can review the app's code to see how it implemented the relevant use case\.




| Amazon Chime SDK | Feature | Demo app resources | 
| --- | --- | --- | 
| JavaScript SDK | Meetings | [Blog post](https://aws.amazon.com/blogs/business-productivity/building-a-meeting-application-using-the-amazon-chime-sdk/), [Demo instructions](https://github.com/aws/amazon-chime-sdk-js/tree/main/demos/serverless), [Source code](https://github.com/aws/amazon-chime-sdk-js/tree/main/demos/browser) | 
| React components | Meetings |   [Demo instructions](https://github.com/aws-samples/amazon-chime-sdk/tree/main/apps/meeting) [Source code](https://github.com/aws-samples/amazon-chime-sdk/tree/main/apps/meeting/src)   | 
| Meeting chat | Messaging |   [Blog post](https://aws.amazon.com/blogs/business-productivity/build-meeting-features-into-your-amazon-chime-sdk-messaging-application/), [Demo instructions](https://github.com/aws-samples/amazon-chime-sdk/tree/main/apps/chat), [Source code](https://github.com/aws-samples/amazon-chime-sdk/tree/main/apps/chat/src)   | 
| iOS/Android | Meetings |  [iOS and Android overview](https://aws.amazon.com/blogs/business-productivity/amazon-chime-sdks-ios-android/) [iOS blog post](https://aws.amazon.com/blogs/business-productivity/building-a-meeting-application-on-ios-using-the-amazon-chime-sdk/)   | 
| PSTN audio | Inbound calling |   [Blog post](https://github.com/aws-samples/amazon-chime-sma-update-call) [Source code](https://github.com/aws-samples/amazon-chime-sma-update-call)   | 

### Using the Meeting Readiness Checker<a name="ready-checker"></a>

Use the [Amazon Chime SDK Meeting Readiness Checker](https://github.com/aws/amazon-chime-sdk-js#meeting-readiness-checker) on GitHub\. The checker helps verify audio and video devices, and user connections\. You can present the results to your end users with by using pass/fail statues that expose the root cause of any issues\.