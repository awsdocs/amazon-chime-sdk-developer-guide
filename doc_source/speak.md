# Speak<a name="speak"></a>

You can play speech on any call leg by providing text\. You can use plain text or Speech Synthesis Markup Language \(SSML\)\. SSML provides more control over how Amazon Chime generates speech by adding pauses, emphasizing certain words, or changing the speaking style\.

Amazon Chime uses the Amazon Polly service to convert text\-to\-speech\. Amazon Polly allows you to choose between either the standard or neural engine for improved speech quality\. Amazon Polly supports more than 20 languages and 60 voices to customize your application's user experience\. Amazon Chime provides speech features at no charge, but you do pay for using Amazon Polly\. See the Amazon Polly [pricing page](https://aws.amazon.com/polly/pricing/) or your billing dashboard for pricing information\.

**Important**  
Use of Amazon Polly is subject to the [ AWS Service Terms ](https://aws.amazon.com/service-terms/), including the terms specific to the AWS Machine Learning and Artificial Intelligence Services\.

**Topics**
+ [Using the Speak action](#speak-action)
+ [Handling ACTION\_SUCCESSFUL events](#speak-action-success)
+ [Handling ACTION\_FAILED events](#speak-action-fail)
+ [Program flows](#speak-flow)

## Using the Speak action<a name="speak-action"></a>

The following example shows a typical use of the `Speak` action\.

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "Speak",
            "Parameters": {
                "Text": "Hello, World!",        // required
                "CallId": "call-id-1",          // required
                "Engine": "neural",             // optional. Defaults to standard
                "LanguageCode": "en-US",        // optional
                "TextType": "text",             // optional
                "VoiceId": "Joanna"             // optional. Defaults to Joanna
            }
        }
    ]
}
```

**CallId**  
*Description* – The `CallId` of participant in the `CallDetails` of the Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – Yes  
*Default value* – None

**Text**  
*Description* – Specifies the input text to synthesize into speech\. If you specify `ssml` as the `TextType`, follow the SSML format for the input text\.  
*Allowed values* – String  
*Required* – Yes  
*Default value* – None

**Engine**  
*Description* – Specifies the engine—standard or neural—to use when processing text for speech synthesis\.  
*Allowed values* – standard \| neural  
*Required* – No  
*Default value* – standard

**LanguageCode**  
*Description* – Specifies the language code\. Only necessary if using a bilingual voice\. If you use a bilingual voice without a language code, the bilingual voice's default language is used\.  
*Allowed values* – [ Amazon Polly language codes](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html#polly-SynthesizeSpeech-request-LanguageCode)  
*Required* – No  
*Default value* – None

**TextType**  
*Description* – Specifies the type of input text, plain text or SSML\. If an input type is not specified, plain text is used as the default\. For more information about SSML, see [Generating Speech from SSML Documents](https://docs.aws.amazon.com/polly/latest/dg/ssml.html) in the *Amazon Polly Developer Guide*\.  
*Allowed values* – ssml \| text  
*Required* – No  
*Default value* – None

**VoiceId**  
*Description* – Specifies the ID of voice you want to use\.  
*Allowed values* – [Amazon Polly voice IDs](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html#polly-SynthesizeSpeech-request-VoiceId)  
*Required* – No  
*Default value* – Joanna

## Handling ACTION\_SUCCESSFUL events<a name="speak-action-success"></a>

The following example shows a typical `ACTION_SUCCESSFUL` event for an action which synthesizes the text "Hello World" into speech, in English, using the Amazon Polly's `Joanna` voice\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
       "Type": "Speak",
       "Parameters": {
          "CallId": "call-id-1",          
          "Engine":  "neural",             
          "LanguageCode":  "en-US",        
          "Text": "Hello World",          
          "TextType":  "text",             
          "VoiceId":  "Joanna"        
       }
    },
    "CallDetails":{       
       ...
    }
}
```

## Handling ACTION\_FAILED events<a name="speak-action-fail"></a>

The following example shows a typical `ACTION_FAILED` event for the same event used in the previous example\.

```
{
    "SchemaVersion": "1.0",
    "Sequence":2,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData":{
       "Type": "Speak",
       "Parameters": {
          "CallId": "call-id-1",          
          "Engine":  "neural",             
          "LanguageCode":  "en-US",        
          "Text": "Hello  World",          
          "TextType":  "text",             
          "VoiceId":  "Joanna"        
       },
       "ErrorType": "SystemException",
       "ErrorMessage": "System error while running  action"
    },
    "CallDetails":{       
       ...
    }
}
```

**Error handling**  
This table lists and describes the error messages thrown by the the `Speak` action\.


| Error | Message | Reason | 
| --- | --- | --- | 
| `AccessDenied` | The `AWSServiceRoleForAmazonChimeVoiceConnector` service\-linked role is not configured correctly\. | The service\-linked role used to make requests to Amazon Polly doesn't exist or is missing permissons\. To resolve, see the steps in the [Using the Amazon Chime Voice Connector service\-linked role](speak-and-get-digits.md#speak-digits-policy) section | 
| `InvalidActionParameter` |   | There was an error validating the action parameters\. See the [SynthesizeSpeech API](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech) in the *Amazon Polly Developer Guide* for more information about parameters\. | 
| ActionExecutionThrottled | Amazon Polly is throttling the request to synthesize speech\. | The request to Amazon Polly is returning a throttling exception\. For more information about the Amazon Polly throttling limits, see [ https://docs\.aws\.amazon\.com/polly/latest/dg/limits\.html\#limits\-throttle ](https://docs.aws.amazon.com/polly/latest/dg/limits.html#limits-throttle)\. | 
| `MissingRequiredActionParameter` | `Text` is a required parameter\. | There action parameters must have a `Text` value | 
| `MissingRequiredActionParameter` | `Text` is limited to 1,000 characters | The text exceeded the character limit\. | 
| `SystemException` | System error while running action\. | A system error occurred while running the action\. | 

## Program flows<a name="speak-flow"></a>

The following diagram shows the program flow that enables the `Speak` action for a caller\. In this example, the caller hears text that 

![\[Diagram showing the program flow for enabling the Speak action for a caller.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/Speak1.png)

**In the diagram**  
Using a soft phone, a caller enters a number registered to a SIP media application\. The application uses the SIP `INVITE` method and sends the caller a `Trying (100)` response\. That indicates that the next\-hop server received the call request\. The SIP application then uses `INVITE` to contact the endpoint\. Once the connection is established, the applications sends `Ringing (180)` response to the caller, and alerting begins\. 

The SIP media application then sends a `NEW_INBOUND_CALL` event to the Lambda function, which responds with a `Speak` action that includes the caller's ID and the text that you want to convert into speech\. The SIP application then sends a `200 (OK)` response to indicate that the call was answered\. The protocol also enables the media\. 

If the `Speak` action succeeds and converts the text to speech, it returns an `ACTION_SUCCESSFUL` event to the SIP media application, which returns the next set of actions\. If the action fails, the SIP media application sends an `ACTION_FAILED` event to the Lambda function, which responds with a set of `Hangup` actions\. The application hangs up the caller and returns a `HANGUP` event to the Lambda function, which takes no further actions\. 



The following diagram shows the program flow than enables the `Speak` action for a callee\.

![\[Diagram showing the program flow for enabling the Speak action for a callee. You can do this on any bridged call.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/Speak2.png)

**In the diagram**  
A caller enters a number registered to a SIP media application, and the application responds as described for the previous diagram\. When the Lambda function receives the `NEW_INBOUND_CALL` event, it returns the [CallAndBridge](call-and-bridge.md) action to the SIP application\. The application then uses the SIP `INVITE` method to send the `Trying (100)` and `Ringing (180)` responses to the callee\. 

If the callee answers, the SIP media application recieves a `200 (OK)` response, and it sends the same response to the caller\. That establishes media, and the SIP application sends an `ACTION_SUCCESSFUL` event for the [CallAndBridge](call-and-bridge.md) action to the Lambda function\. The function then returns the Speak action and data to the SIP application, which converts 

