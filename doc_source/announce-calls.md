# Announcing skill calls<a name="announce-calls"></a>

Your Alexa Skill must announce the start of a skill call\. Your users must know when they stop speaking with Alexa and start speaking with your telephony application\.

To announce a call, your Skill returns the call announcement text in the `outputSpeech` field of the Skill's response\. For more information about responses, see [Response Format](https://developer.amazon.com/en-US/docs/alexa/custom-skills/request-and-response-json-reference.html#response-format) in the *Alexa Skills Kit*\.

For example, say you have a telephony app named "Example corp\." You can name your Skill “Calling Example corp\.” For more information on requests and responses for an Alexa Skill, see the [ Request and Response JSON Reference ](https://developer.amazon.com/en-US/docs/alexa/custom-skills/request-and-response-json-reference.html) in the *Alexa Skills Kit*\.