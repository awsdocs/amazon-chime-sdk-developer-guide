# Building an Alexa Skill with skill calling<a name="build-skill-with-calling"></a>

Alexa Skills contain an interaction model and application logic\. The interaction model provides the voice user interface\. When a caller speaks, Alexa processes the speech in the context of your interaction model to determine the customer's request\. Alexa then sends the request to your Skill's application logic, which acts on the request\. You provide your application logic as an AWS Lambda function\. For more information on building an Alexa Skill, see Alexa [Custom Skills](https://developer.amazon.com/en-US/alexa/alexa-skills-kit/get-deeper/custom-skills) in the *Alexa Skills Kit*\.

When interacting with your Skill, the caller instructs it to start a call\. Your application logic then uses the Alexa service's `StartCommunicationSession` API to start a call to your SIP media application\. The following topics explain the details of the process\.

## Using the StartCommunicationSession API<a name="start-comm-session"></a>

The following example shows a typical `StartCommunicationSession` request\. The request starts a communication session within an Alexa Skill\.

A valid communication session must have two participants\. One of the participants must be the originator of the session\. The other participant must be a non\-originator\.

```
POST /v1/communications/session HTTP/1.1 
Authorization: Bearer AuthorizationToken
Content-type: application/json

{
  "participants": [
    {
      "id": {
        "type": "PHONE_NUMBER",
        "value": "+12045551111"
      },
      "endpointId": "amzn1.ask.device.BHOSHBLLX53AFDC5KSPFX3IM3NZKLVAHCG3CPUDM242MB55ID3OB5XUYQO332QUZ5FY45Z7RBR6RJD7ZKUSWLP5SJ6ZREGLKLBVY2DZKPUCTFEJE55JV6DWMAAO7UUNCIKGWCK6FWUS75F5LWBOKHXARBZYXQDHS6OCY7O6JBAKBDJRELAR7O",
      "isOriginator": true
    },
    {
      "id": {
        "type": "PHONE_NUMBER",
        "value": "+16073331111"
      },
      "communicationProviderId": "amzn1.alexa.csp.id.82bb98bc-384a-11ed-a261-0242ac120002"
    }
  ],
  "clientContext": {
    "clientSessionId": "db792233-8df3-416c-8d80-a70038747b74"
  }
}
```

The HTTP request must contain the `Authorization` and `Content-type` request headers\.

The authorization header uses `Authorization` as the key, and `Bearer` *AuthorizationToken* as the values\. In turn, `AuthorizationToken` is the `apiAccessToken` in the [Alexa Skill request](https://developer.amazon.com/en-US/docs/alexa/custom-skills/request-and-response-json-reference.html) provided to the Alexa Skill's AWS Lambda function\. 

The content type header always uses `Content-type` as the key, and `application/json` as the value\.

Always set the `isOriginator` field of the originating participant to `true`\. In this example, the originator used `+12045551111`\. Phone numbers must use the E\.164 format\. The originator must also provide the endpoint, the `deviceId` in the [Alexa Skill request](https://developer.amazon.com/en-US/docs/alexa/custom-skills/request-and-response-json-reference.html) provided to the AWS Lambda function\.

The Skill must provide the Alexa user's phone number in the originator’s `ParticipantId` field\. You can configure your Skill to request the customer’s permission to access their phone number\. Once the customer grants the permission, your Skill can then use that number in the skill call\. For more information, see [ Request Customer Contact Information for Use in Your Skill](https://developer.amazon.com/en-US/docs/alexa/custom-skills/request-customer-contact-information-for-use-in-your-skill.html)\. As an alternative, if the Skill supports account linking, the Alexa user can link their customer profile to the Skill\. The Skill can then use the phone number from the Alexa user's profile\. For more information, see [ Add Account Linking to Your Alexa Skill](https://developer.amazon.com/en-US/docs/alexa/account-linking/add-account-linking.html)\.

The `isOriginator` field of the non\-originator participant is optional\. But, if you specify `isOriginator`, you must set it to `false`\. To start a communication session with a phone number, the `communicationProviderId` value must be `amzn1.alexa.csp.id.82bb98bc-384a-11ed-a261-0242ac120002`\.

The Skill must provide its associated phone number in the non\-originator’s `ParticipantId`\. The phone number must be an Amazon Chime SDK phone number of the SIP media application dial\-in type\. The phone number must be associated with a SIP media application, and you must configure that application to allow calls from the Skill\.

The user\-to\-user header in the `SIP INVITE` provides the `clientSessionId` value\. The ID value consists of a randomly generated identifier\. We strongly recommended using unique identifiers in all Skills for each communication session\.

For more information about user\-to\-user data, see the [IETF's RFC 7433](https://datatracker.ietf.org/doc/html/rfc7433)\.