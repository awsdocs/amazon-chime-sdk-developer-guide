# SpeakAndGetDigits<a name="speak-and-get-digits"></a>

Play speech by providing text and gather dual tone multi\-frequency \(DTMF\) digits from the user\. The text can either be plain text or Speech Synthesis Markup Language \(SSML\)\-enhanced text to provide more control over how Amazon Chime generates speech by adding pauses, emphasizing certain words, or changing the speaking style, among other supported SSML features\. If a failure occurs, such as a user not entering the correct number of DTMF digits, the action plays the "failure" speech and then replays the main speech until the SIP media application exhausts the number of attempts defined in the `Repeat` parameter\.

Amazon Chime uses Amazon Polly, a cloud service that converts text into lifelike speech, Amazon Polly provides both a standard and a neural engine for improved speech quality, more than 20 supported languages, and 60 voices\. Amazon Polly provides speech features at no charge, but you do pay for using Amazon Polly\. See the Amazon Polly [pricing page](https://aws.amazon.com/polly/pricing/) or your billing dashboard for pricing information\.

**Important**  
Use of Amazon Polly is subject to the [AWS Service Terms ](https://aws.amazon.com/service-terms/), including the terms specific to the AWS Machine Learning and Artificial Intelligence Services\.

**Topics**
+ [Using the SpeakAndGetDigits action](#speak-digits-action)
+ [Handling ACTION\_SUCCESSFUL events](#speak-digits-success)
+ [Handling ACTION\_FAILED events](#speak-digits-fail)
+ [Using the Amazon Chime Voice Connector service\-linked role](#speak-digits-policy)

## Using the SpeakAndGetDigits action<a name="speak-digits-action"></a>

The following example shows a typical use of the `SpeakAndGetDigits` action:

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "SpeakAndGetDigits",
            "Parameters": {
                "CallId": "call-id-1",          // required
                "InputDigitsRegex": "^\d{2}#$", // optional
                "SpeechParameters": {
                    "Text": "Hello World",      // required
                    "Engine": "neural",         // optional. Defaults to standard
                    "LanguageCode": "en-US",    // optional
                    "TextType": "text",         // optional
                    "VoiceId": "Joanna"         // optional. Defaults to Joanna
                },
                "FailureSpeechParameters": {
                    "Text": "Hello World",      // required
                    "Engine": "neural",         // optional. Defaults to the Engine value in SpeechParameters
                    "LanguageCode": "en-US",    // optional. Defaults to the LanguageCode value in SpeechParameters
                    "TextType": "text",         // optional. Defaults to the TextType value in SpeechParameters
                    "VoiceId": "Joanna"         // optional. Defaults to the VoiceId value in SpeechParameters
                },
                "MinNumberOfDigits": 3,         // optional
                "MaxNumberOfDigits": 5,         // optional
                "TerminatorDigits": ["#"],      // optional
                "InBetweenDigitsDurationInMilliseconds": 5000,  // optional
                "Repeat": 3,                    // optional
                "RepeatDurationInMilliseconds": 10000           // required
            }
        }
    ]
}
```

**CallId**  
*Description* – The `CallId` of participant in the CallDetails of the Lambda function invocation\.  
*Allowed values* – A valid `callID`  
*Required* – Yes  
*Default value* – No

**InputDigitsRegex**  
*Description* – A regular expression pattern to help ensure that users enter the correct digits and letters\.  
*Allowed values* – A valid regular expression pattern  
*Required* – No  
*Default value* – None

**SpeechParameters\.Engine**  
*Description* – Specifies the engine – standard or neural – to use when processing text for speech synthesis\.  
*Allowed values* – `standard` \| `neural`  
*Required* – No  
*Default value* – Standard

**SpeechParameters\.LanguageCode**  
*Description* – Specifies the language code\. This is only necessary if using a bilingual voice\. If a bilingual voice is used and no language code is specified, the bilingual voice's default language is used\.  
*Allowed values* – [ Amazon Polly language codes](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html#polly-SynthesizeSpeech-request-LanguageCode)  
*Required* – No  
*Default value* – None

**SpeechParameters\.Text**  
*Description* – Specifies the input text\. If you specify `ssml` as the `SpeechParameters.TextType`, you must follow the SSML format for the input text\. For more information about SSML, see [Generating Speech from SSML Documents](https://docs.aws.amazon.com/polly/latest/dg/ssml.html) in the *Amazon Polly Developer Guide*\.  
*Allowed values* – String  
*Required* – Yes  
*Default value* – None

**SpeechParameters\.TextType**  
*Description* – Specifies the text format for `SpeechParameters.Text`\. If not specified, `text` is used by default\. For more information about SSML, see [Generating Speech from SSML Documents](https://docs.aws.amazon.com/polly/latest/dg/ssml.html) in the *Amazon Polly Developer Guide*\.  
*Allowed values* – `ssml` \| `text`  
*Required* – No  
*Default value* – `text`

**SpeechParameters\.VoiceId**  
*Description* – The ID of the Amazon Polly voice used to speak the text in `SpeechParameters.Text`\.  
*Allowed values* – [Amazon Polly voice IDs](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html#polly-SynthesizeSpeech-request-VoiceId)  
*Required* – No  
*Default value* – Joanna

**FailureSpeechParameters\.Engine**  
*Description* – Specifies the engine – standard or neural – to use when processing the failure message used when the customer enters an invalid response for speech synthesis\.  
*Allowed values* – `standard` \| `neural`  
*Required* – No  
*Default value* – The `SpeechParameters.Engine` value

**FailureSpeechParameters\.LanguageCode**  
*Description* – Specifies the language code used when the customer enters an invalid response\. Only necessary when using a bilingual voice\. If you use bilingual voice without specifying a language code, the bilingual voice's default language is used\.  
*Allowed values* – [ Amazon Polly language codes](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html#polly-SynthesizeSpeech-request-LanguageCode)  
*Required* – No  
*Default value* – The `SpeechParameters.LanguageCode` value\.

**FailureSpeechParameters\.Text**  
*Description* – Specifies the input text spoken when the customer enters an invalid response\. If you specify `ssml` as the `FailureSpeechParameters.TextType`, you must follow the SSML format for the input text\.  
*Allowed values* – String  
*Required* – Yes  
*Default value* – None

**FailureSpeechParameters\.TextType**  
*Description* – Specifies whether the input text specified in `FailureSpeechParameters.Text` is plain text or SSML\. The default value is plain text\. For more information, see [Generating Speech from SSML Documents](https://docs.aws.amazon.com/polly/latest/dg/ssml.html) in the *Amazon Polly Developer Guide*\.  
*Allowed values* – `ssml` \| `text`  
*Required* – No  
*Default value* – The `SpeechParameters.Text` value

**FailureSpeechParameters\.VoiceId**  
*Description* – The ID for the voice used to speak the string in `FailureSpeechParameters.Text`\.  
*Allowed values* – [Amazon Polly voice IDs](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html#polly-SynthesizeSpeech-request-VoiceId)  
*Required* – Yes  
*Default value* – The `SpeechParameters.VoiceId` value

**MinNumberOfDigits**  
*Description* – The minimum number of digits to capture before timing out or playing the "call failed" message\.  
*Allowed values* – Greater than or equal to zero  
*Required* – No  
*Default value* – 0

**MaxNumberOfDigits**  
*Description* – The maximum number of digits to capture before stopping without a terminating digit\.  
*Allowed values* – Greater than `MinNumberOfDigits`  
*Required* – No  
*Default value* – 128

**TerminatorDigits**  
*Description* – Digit used to end input if the user enters less than the MaxNumberOfDigits  
*Allowed values* – Any one of: 0 1 2 3 4 5 6 7 8 9 \# or \*  
*Required* – No  
*Default value* – \#

**InBetweenDigitsDurationInMilliseconds**  
*Description* – The wait time in milliseconds between digit inputs before playing the failure speech\.  
*Allowed values* – Greater than zero  
*Required* – No  
*Default value* – If not specified, defaults to the `RepeatDurationInMilliseconds` value

**Repeat**  
*Description* – Total number of attempts to get digits\. If you omit this parameter, the default is one attempt to collect digits\.  
*Allowed values* – Greater than zero  
*Required* – No  
*Default value* – 1

**RepeatDurationInMilliseconds**  
*Description* – Timeout in milliseconds for each attempt to get digits\.  
*Allowed values* – Greater than zero  
*Required* – Yes  
*Default value* – None

## Handling ACTION\_SUCCESSFUL events<a name="speak-digits-success"></a>

The following example shows a typical `ACTION_SUCCESSFUL` event\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
            "Type":  "SpeakAndGetDigits",
            "Parameters": {
                "CallId": "call-id-1",          
                "InputDigitsRegex":  "^\d{2}#$", 
                "SpeechParameters": {
                    "Engine":  "neural",         
                    "LanguageCode": "en-US",    
                    "Text":  "Hello World",           
                    "TextType":  "text",         
                    "VoiceId": "Joanna"         
                },
                "FailureSpeechParameters": {
                    "Engine":  "neural",         
                    "LanguageCode":  "en-US",    
                    "Text":  "Hello World",           
                    "TextType": "text",         
                    "VoiceId": "Joanna"         
                },
                "MinNumberOfDigits": 3,         
                "MaxNumberOfDigits": 5,         
                "TerminatorDigits": ["#"],      
                "InBetweenDigitsDurationInMilliseconds": 5000,  
                "Repeat": 3,                    
                "RepeatDurationInMilliseconds": 10000           
            },
            "ReceivedDigits": "1234"
    },
    "CallDetails":{       
       ...
    }
}
```

## Handling ACTION\_FAILED events<a name="speak-digits-fail"></a>

The following example shows a typical ACTION\_FAILED event\.

```
{
    "SchemaVersion": "1.0",
    "Sequence":2,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData":{
            "Type":  "SpeakAndGetDigits",
            "Parameters": {
                "CallId": "call-id-1",          
                "InputDigitsRegex":  "^\d{2}#$", 
                "SpeechParameters": {
                    "Engine":  "neural",         
                    "LanguageCode": "en-US",    
                    "Text":  "Hello World",           
                    "TextType":  "text",         
                    "VoiceId": "Joanna"         
                },
                "FailureSpeechParameters": {
                    "Engine":  "neural",         
                    "LanguageCode":  "en-US",    
                    "Text":  "Hello World",          
                    "TextType": "text",        
                    "VoiceId": "Joanna"        
                },
                "MinNumberOfDigits": 3,      
                "MaxNumberOfDigits": 5,        
                "TerminatorDigits": ["#"],      
                "InBetweenDigitsDurationInMilliseconds": 5000,  
                "Repeat": 3,                    
                "RepeatDurationInMilliseconds": 10000         
            },
            "ErrorType":  "SystemException",
            "ErrorMessage":  "System error while executing action"
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
| `AccessDenied` | The `AWSServiceRoleForAmazonChimeVoiceConnector` role is not configured correctly\. | The role used to make requests to Amazon Polly doesn't exist or is missing permissons\. To resolve, see the steps in the [Using the Amazon Chime Voice Connector service\-linked role](#speak-digits-policy) section | 
| `InvalidActionParameter` |   | There was an error validating the action parameters\. To review the available parameters for this action, and their options, see [https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html](https://docs.aws.amazon.com/polly/latest/dg/API_SynthesizeSpeech.html) in the Amazon Polly Developer Guide\. | 
| `MissingRequiredActionParameter` | `Text` is a required parameter\. | The action parameters must have a `Text` value | 
| `MissingRequiredActionParameter` | `Text` is limited to 1,000 characters | The text exceeded the character limit\. | 
| `SystemException` | System error while running action\. | A system error occurred while running the action\. | 

## Using the Amazon Chime Voice Connector service\-linked role<a name="speak-digits-policy"></a>

You don't need to manually create a service\-linked role for the `Speak` or `SpeakAndGetDigits` actions\. When you create or update a SIP media application in the Amazon Chime console, the AWS Command Line Interface, or the AWS API, Amazon Chime creates the service\-linked role for you\.

For more information, see [ Using the Amazon Chime service\-linked role](https://docs.aws.amazon.com/chime/latest/ag/using-service-linked-roles-stream.html) in the *Amazon Chime Administrator Guide*\.