# Debugging and troubleshooting<a name="debug-pstn"></a>

Use the following information to help you diagnose and fix common issues that you might encounter when working with the Amazon Chime SDK PSTN Audio service\.

**Topics**
+ [Checking the logs](#check-logs)
+ [Debugging unexpected hangups](#unexpected-hangups)
+ [Debugging unexpected ACTION\_FAILED events](#unexpected-action-fail)

## Checking the logs<a name="check-logs"></a>

If you're debugging a SIP media application, check the Cloudwatch logs for the AWS Lambda function associated with the application\. 

Next, check the logs associated with the SIP media application\. As needed, you can configure the SIP media application for logging\. For more information, see [Using SIP media applications](https://docs.aws.amazon.com/chime-sdk/latest/ag/use-sip-apps.html) in the *Amazon Chime SDK Administrator Guide*\. If you enable logging, you can find the logs on Cloudwatch, in the /aws/ChimeSipMediaApplicationSipMessages/*SIP media application Id* log group\.

## Debugging unexpected hangups<a name="unexpected-hangups"></a>


+ Verify that your AWS Lambda policy grants the **lambda:InvokeFunction** permission to the [voiceconnector\.chime\.amazonaws\.com](http://voiceconnector.chime.amazonaws.com/) service principal\.
+ Check the logs for your AWS Lambda function to ensure that it's being successfully invoked\.
+ If the logs show incoming events and returned actions, verify that you don't return a hangup action in when the AWS Lambda function is invoked\.
+ Check the Cloudwatch logs for your SIP media application\. The following table below provides information about some of the messages you may encounter\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/debug-pstn.html)

## Debugging unexpected ACTION\_FAILED events<a name="unexpected-action-fail"></a>

If you receive an unexpected `ACTION_FAILED` event, check the following:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/debug-pstn.html)