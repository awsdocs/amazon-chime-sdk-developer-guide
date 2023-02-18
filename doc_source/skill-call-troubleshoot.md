# Troubleshooting Amazon Chime SDK Alexa skill calling<a name="skill-call-troubleshoot"></a>

The topics in this section explain how to troubleshoot errors in Amazon Chime SDK Alexa skill calling\. Expand the sections as needed\.

## Alexa replies with, "The contact is not available"<a name="contact-not-available"></a>

Enable Amazon Chime SDK Alexa skill calling for your SIP media application\. To do that, follow the steps in [ Enabling Amazon Chime SDK Alexa Skill calling ](https://docs.aws.amazon.com/chime-sdk/latest/ag/enable-alexa-calling.html) in the *Amazon Chime SDK Administrator Guide*\.

If you see this error after you enable skill calling, check the CloudWatch logs for the AWS Lambda function associated with the application\. For more information, see [Checking the logs](check-logs.md) in the *Debugging and troubleshooting* section of this guide\.

## The StartCommunicationSession API returns a 404 for Alexa Europe and Far East endpoints<a name="startcomm-404"></a>

Alexa skill calling is only available for Alexa accounts in North America\. When you invoke the [StartCommunicationSession](communication-session-reference.md#start-communication-session) API using an Alexa Europe or Far East endpoint, the API returns a 404 error\.