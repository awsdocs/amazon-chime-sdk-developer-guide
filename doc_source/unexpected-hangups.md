# Debugging unexpected hangups<a name="unexpected-hangups"></a>


+ Verify that your AWS Lambda policy grants the `lambda:InvokeFunction` permission to the [voiceconnector\.chime\.amazonaws\.com](http://voiceconnector.chime.amazonaws.com/) service principal\.
+ Check the logs for your AWS Lambda function to ensure that it's being successfully invoked\.
+ If the logs show incoming events and returned actions, verify that you don't return a hangup action in when the AWS Lambda function is invoked\.
+ Check the Cloudwatch logs for your SIP media application\. The following table lists some of the messages you may encounter\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/unexpected-hangups.html)