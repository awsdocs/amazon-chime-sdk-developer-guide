# Using Amazon Chime SDK Alexa skill calling<a name="alexa-calling"></a>

Amazon Chime SDK Alexa skill calling allows enterprise customers to enable calling directly in their Amazon Alexa Skills\. Alexa Skills\. For example, a customer can say, "Alexa, call Example corporation customer support\." The trigger phrase "Alexa" tells Alexa to start listening to the user\. Saying, "call Example corporation" starts the Skill, and saying, "customer support" triggers the skill\-calling functionality\.

For more information about invoking a custom Alexa Skill, see [ Understanding How Users Invoke Custom Skills](https://developer.amazon.com/en-US/docs/alexa/custom-skills/understanding-how-users-invoke-custom-skills.html)

**Billing for Alexa skill calling**  
AWS bills you for the duration of the Alexa skill call and the duration of any Amazon Chime SDK PSTN audio usage per attendee\. For more information, see [Amazon Chime SDK pricing](https://aws.amazon.com/chime/chime-sdk/pricing/)\.

The following topics explain how to add Alexa skill calling to an Alexa Skill\.

**Topics**
+ [Overview](alexa-overview.md)
+ [Building an Alexa Skill with skill calling](build-skill-with-calling.md)
+ [Using clientSessionId to send call context data](call-context-data.md)
+ [Announcing skill calls](announce-calls.md)
+ [Handling skill call events](handle-skill-call-events.md)
+ [Use cases for skill calls](alexa-use-cases.md)
+ [StartCommunicationSession API reference](communication-session-reference.md)