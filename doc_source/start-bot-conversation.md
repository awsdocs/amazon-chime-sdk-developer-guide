# StartBotConversation<a name="start-bot-conversation"></a>

The `StartBotConversation` action establishes a voice conversation between an end user and your Amazon Lex v2 bot\. The user provides the required information to the bot\. The bot then returns the information to the public switched telephone network \(PSTN\) Audio Lambda function, and the function performs the requested tasks\.

For example, the bot can play an optional welcome message at the start of a conversation to briefly describe the task that the PSTN Audio Lambda function can perform\. The conversation goes back and forth between the user and the bot until the bot gathers the required information\. Once the conversation ends, the Amazon Chime SDK invokes your PSTN Audio Lambda function with an action success event, which contains the information gathered by the bot\. Your PSTN Audio Lambda function processes the information and performs the requested task\.

The PSTN Audio service provides life\-like conversational interaction with your users\. For example, users can interrupt the bot and answer a question before the audio prompt finishes\. What's more, users can use any combination of voice and DTMF digits to provide information\. The bot waits for the user to provide input before responding\. You can configure how long the bot waits for the user to finish speaking before interpreting any speech input\. The user can also instruct the bot to wait if they need time to retrieve additional information during a call, such as credit card numbers\.

The `StartBotConversation` action uses Amazon Lex and Amazon Polly for the duration of the bot conversation\. Standard Amazon Lex and Amazon Polly costs apply\. For more pricing information, see the [Amazon Lex streaming conversation pricing](https://aws.amazon.com/lex/pricing/), and [Amazon Polly Pricing](https://aws.amazon.com/polly/pricing/) pages\.

**Note**  
You can't run this action on a bridged call, or on a call that has joined an Amazon Chime meeting\.

**Important**  
Use of Amazon Lex and Amazon Polly is subject to the [AWS Service Terms ](https://aws.amazon.com/service-terms/), including the terms specific to the AWS Machine Learning and Artificial Intelligence Services\.

**Topics**
+ [StartBotConversation syntax](#startbot-syntax)
+ [Using the StartBotConversation action](#using-startbot)
+ [Handling ACTION\_SUCCESSFUL events](#bot-action-success)
+ [Handling ACTION\_FAILED events](#bot-action-fail)
+ [Granting permissions to use a bot](#bot-permissions)
+ [Configuring voice and DTMF timeouts](#bot-timeouts)
+ [Using DTMF inputs during a conversation](#bot-dtmf)
+ [Billing and service quotas](#bot-billing)

## StartBotConversation syntax<a name="startbot-syntax"></a>

The following example shows typical `StartBotConversation` syntax\.

```
{
  "SchemaVersion": "1.0",
  "Actions":[
    {
      "Type": "StartBotConversation",
      "Parameters": {
        "CallId": "string",
        "ParticipantTag": "string",
        "BotAliasArn": "string",
        "LocaleId": "string",
        "Configuration": {
          "SessionState": {
             "SessionAttributes": {
                "string": "string" 
             },
             "DialogAction" : {
               "Type": "string"
             }
          },
          "WelcomeMessages": [
            {
              "Content": "string",
              "ContentType": "string" 
            }
          ]
        }
      }
    }
  ]
}
```

**CallId**  
*Description* – The `CallID` of a participant in the `CallDetails` of the AWS Lambda function invocation\. The `StartBotConversation` action uses this ID as the bot's `SessionId`\. All bot conversations that take place on a call share the same conversation session\. You can modify the session state between your user and your bot by using the [ Amazon Lex PutSession ](https://docs.aws.amazon.com/lexv2/latest/dg/API_runtime_PutSession.html) API\. For more information, see [ Managing sessions with the Amazon Lex v2 API ](https://docs.aws.amazon.com/lexv2/latest/dg/using-sessions.html) in the *Amazon Lex Developer Guide*\.  
*Allowed values* – A valid call ID  
*Required* – No, if `ParticipantTag` is present  
*Default value* – None

**ParticipantTag**  
*Description* – The `ParticipantTag` of one of the connected participants in the `CallDetails`\.  
*Allowed values* – `LEG-A`  
*Required* – No, if `CallId` is present  
*Default value* – `ParticipantTag` of the invoked `callLeg`\. Ignored if you specify `CallDetails`

**BotAliasArn**  
*Description* – The bot alias ARN of your Lex bot\. You must create the bot in the same AWS Region as your PSTN Audio application\. A valid Amazon Lex bot alias has this format: `arn:aws:lex:region:awsAccountId:bot-alias/botId/botAliasId`, where *`region`* is the AWS Region where your bot resides\. The `awsAccountId` is the AWS account ID in which your Amazon Lex bot is created\. The `botId` value is the identifier assigned to the bot when you created it\. You can find the bot ID in the Amazon Lex console on the **Bot details** page\. The `botAliasId` is the identifier assigned to the bot alias when you created it\. You can find the bot alias ID in the Amazon Lex console on the **Aliases** page\.   
*Allowed values* – A valid bot ARN  
*Required* –Yes  
*Default value* –None

**LocaleId**  
*Description* – The identifier of the locale that you used for your bot\. For a list of locales and language codes, see [ Languages and locales supported by Amazon Lex ](https://docs.aws.amazon.com/lexv2/latest/dg/how-languages.html)\.  
*Allowed values* – [ Languages and locales supported by Amazon Lex ](https://docs.aws.amazon.com/lexv2/latest/dg/how-languages.html)  
*Required* – No  
*Default value* – `en_US`

**Configuration**  
*Description* – The conversation configuration, including session state and welcome messages\. The total size of the JSON string representation of the `Configuration` object is limited to 10 KB\.  
*Allowed values* – `Configuration` object  
*Required* – No  
*Default value* – None

**Configuration\.SessionState**  
*Description* – The state of the user's session with Amazon Lex v2\.  
*Allowed values* – `SessionState` object  
*Required* – No  
*Default value* – None

**Configuration\.SessionState\.SessionAttributes**  
*Description* – A map of the key/value pairs that represent session\-specific context information\. This map contains application information passed between Amazon Lex v2 and a client application\.  
*Allowed values* – String to string map  
*Required* – No  
*Default value* – None

**Configuration\.SessionState\.DialogAction\.Type**  
*Description* – The next action that the bot takes in its interactions with the user\. Possible values:  
+ *Delegate* Amazon Lex v2 determines the next action\.
+ *ElicitIntent* The next action elicits an intent from the user\.
*Allowed values* – `Delegate` \| `ElicitIntent`  
*Required* – No  
*Default value* – None

**Configuration\.WelcomeMessages**  
*Description* – A list of messages to send to the user at the start of the conversation\. If you set the `welcomeMessage` field, you must set the `DialogAction.Type` value to `ElicitIntent`\.  
*Allowed values* – Message object  
*Required* – No  
*Default value* – None

**Configuration\.WelcomeMessages\.Content**  
*Description* – The welcome message text\.  
*Allowed values* – String  
*Required* – No  
*Default value* – None

**Configuration\.WelcomeMessages\.ContentType**  
*Description* – Indicates the welcome message type\.  
*Allowed values* –` PlainText` \| `SSML`  
+ *PlainText* – The message contains plain UTF\-8 text\. 
+ *SSML* – The message contains text formatted for voice output\.
*Required* – Yes  
*Default value* – None

## Using the StartBotConversation action<a name="using-startbot"></a>

The following example shows a typical `StartBotConversation` action\.

```
{
  "SchemaVersion": "1.0",
  "Actions":[
    {
      "Type": "StartBotConversation",
      "Parameters": {
        "CallId": "call-id-1",
        "BotAliasArn": "arn:aws:lex:us-east-1:123456789012:bot-alias/ABCDEFGHIH/MNOPQRSTUV",
        "LocaleId": "en_US",
        "Configuration": {
          "SessionState": {
             "SessionAttributes": {
                "mykey1": "myvalue1" 
             },
             "DialogAction" : {
               "Type": "ElicitIntent"
             }
          },
          "WelcomeMessages": [
            {
              "Content": "Welcome. How can I help you?",
              "ContentType": "PlainText" 
            }
          ]
        }
      }
    }
  ]
}
```

## Handling ACTION\_SUCCESSFUL events<a name="bot-action-success"></a>

The following example shows a typical `ACTION_SUCCESSFUL` event for the `StartBotConversation` action\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": number,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData":
    {
        "CallId": "string",
        "Type": "StartBotConversation",
        "Parameters": {
            // parameters provided in the StartBotConversation action.
        },
        "CallDetails": {
            // Information about the call associated with the AWS Lambda invocation.
        },
        "IntentResult": {
            "SessionId": "string",
            "SessionState": {
                "SessionAttributes": {
                    "string": "string"
                },
                "Intent": {
                    "Name": "string",
                    "Slots":  {
                        "string": {
                            "Value": {
                                "OriginalValue": "string",
                                "InterpretedValue": "string",
                                "ResolvedValues": ["string"]
                            },
                            "Values": []
                        }
                    },
                    "State": "string",
                    "ConfirmationState": "string"
                }
            },
            "Interpretations": [
                {
                    "NluConfidence": {
                        "Score": number
                    },
                    "Intent": {
                        "Name": "string",
                        "Slots": {
                            "string": {
                                "Value": {
                                    "OriginalValue": "string",
                                    "InterpretedValue": "string",
                                    "ResolvedValues": ["string"]
                                },
                                "Values": []
                            }
                        },
                        "State": "string",
                        "ConfirmationState": "string"
                    }
                }
            ]
        }
    }
}
```

**IntentResult**  
The result of the conversation between the user and the bot\.

**SessionId**  
The identifier of the bot conversation session\. When a user starts a conversation with your bot, Amazon Lex creates a session\. A session encapsulates the information exchanged between your user and the bot\. The `StartBotConversation` action uses the call ID as the bot's `SessionId`\. You can modify the session state between your user and your bot by using the Lex [ PutSession ](https://docs.aws.amazon.com/exv2/latest/dg/API_runtime_PutSession.html) API\. For more information, see [ Managing sessions with the Amazon Lex V2 API ](https://docs.aws.amazon.com/lexv2/latest/dg/using-sessions.html) in the *Amazon Lex Developer Guide*\.

**SessionState**  
The state of the user’s Amazon Lex v2 session\. 

**SessionState\.SessionAttributes**  
Map of key/value pairs that represent session\-specific context information\. The map contains bot conversation information passed between the Lambda function attached to your bot and the PSTN Audio Lambda function\.

**Interpretations**  
A list of intents derived by Amazon Lex that may satisfy the a customer's utterance\. The intent with the highest `NluConfidence` score becomes the Intent for the `SessionState`\. 

**Interpretations\.NluConfidence\.Score**  
A score that indicates how confident Amazon Lex v2 is that an intent satisfies a user's intent\. Ranges between 0\.00 and 1\.00\. Higher scores indicate higher confidence\. 

**Intent**  
The action the user wants to perform\.

**Intent\.Name**  
The name of the intent\.

**Intent\.Slots**  
A map of all of the slots for the intent\. The name of the slot maps to the value of the slot\. If a slot has not been filled, the value is null\.

**Intent\.Slots\.Value**  
The value of the slot\.

**Intent\.Slots\.Values**  
A list of one or more values that the user provided for the slot\.

**Intent\.Slots\.Value\.OriginalValue**  
The text of the user's reply, entered for the slot\.

**Intent\.Slots\.Value\.InterpretedValue**  
*Description* – The value that Amazon Lex v2 determines for the slot\. The actual value depends on the bot's value selection strategy setting\. You can choose to use the value entered by the user, or you can have Amazon Lex v2 choose the first value in the `resolvedValues` list\.

**Intent\.Slots\.Value\.ResolvedValues **  
A list of additional values that Amazon Lex v2 recognizes for the slot\.

**Intent\.State**  
*Description* – Fulfillment information for the intent\. Possible values:  
+ *Failed* – The Lambda function failed to fulfill the intent\.
+ *Fulfilled* – The Lambda function fulfilled the intent\.
+ *ReadyForFulfillment* – The information for the intent is present, and your Lambdafunction can fulfill the intent\. 

**Intent\.ConfirmationState**  
*Description* – Indicates confirmation of the intent\. Possible values:  
+ *Confirmed* – The Intent is fulfilled\.
+ *Denied* – The user responded "no" to the confirmation prompt\.
+ *None* – The user wasn't prompted for confirmation, or the user was prompted but didn'tconfirm or deny the prompt\.

## Handling ACTION\_FAILED events<a name="bot-action-fail"></a>

The following example shows a typical `ACTION_FAILED` event for the `StartBotConversation` action\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": number,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData":{
        "CallId": "string",
        "Type": "StartBotConversation",
        "Parameters": {
            // parameters provided in the StartBotConversation action
        },
        "ErrorType": "string",
        "ErrorMessage": "string"
    },
    "CallDetails":{
    }
}
```

**ErrorType**  
A string that uniquely identifies an error condition\.

**ErrorMessage**  
A generic description of the error condition\.

### Error codes<a name="action-errors"></a>

The following table lists the error messages that a Lambda function can return in an `ACTION_FAILED` event\.


| Error | Description | 
| --- | --- | 
|  `InvalidActionParameter` | One or more action parameters are invalid\. The error message describes the invalid parameter\. | 
| `SystemException` | A system error occurred while running an action\. | 
| `ResourceNotFound` | A specified bot is not found\. | 
| `ResourceAccessDenied` | Access to the bot is denied\. | 
| `ActionExecutionThrottled` | Bot conversation service limit is exceeded\. The error message describes the specific service limit exceeded\. | 

## Granting permissions to use a bot<a name="bot-permissions"></a>

The following example grants Amazon Chime permission to call the Amazon Lex [StartConversation](https://docs.aws.amazon.com/lexv2/latest/dg/API_runtime_StartConversation.html) APIs\. You must explicitly grant the PSTN Audio service permission to use your bot\. The condition block is required for service principals\. The condition block must use the global context keys `AWS:SourceAccount` and `AWS:SourceArn`\. The `AWS:SourceAccount` is your AWS account ID\. The `AWS:SourceArn` is the resource ARN of the PSTN Audio application that invokes the Lex bot\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowChimePstnAudioUseBot",
      "Effect": "Allow",
      "Principal": {
        "Service": "voiceconnector.chime.amazonaws.com"
      },
      "Action": "lex:StartConversation",
      "Resource": "arn:aws:lex:region:awsAccountId:bot-alias/botId/aliasId",
      "Condition": {
        "StringEquals": {
          "AWS:SourceAccount": "awsAccountId"
        },
        "ArnEquals": {
        "AWS:SourceArn": "arn:aws:voiceconnector:region:awsAccountId:sma/smaId"
        }
      }
    }
  ]
}
```

## Configuring voice and DTMF timeouts<a name="bot-timeouts"></a>

You can configure the voice and DTMF timeouts when capturing user input\. You can configure timeouts through session attributes when starting a conversation with a bot, and overwrite them in your Lex bot's Lambda function if necessary\. Amazon Lex lets you set multiple slots for an intent or bots\. Because you can specify that session attributes apply to the intent and slot level, you can specify that the attribute is set only when you're collecting a certain type of input\. For example, you can specify a longer time\-out when you're collecting an account number than when you're collecting a date\. You can use wildcards in the session attribute key\. 

For example, to set a voice timeout for all slots for all intents to 4000 milliseconds, you can provide a session attribute using: `x-amz-lex:start-timeout-ms:*:*` as the session attribute name and `4000` as the session attribute value\. For more information, see [ Configuring timeouts for capturing user input ](https://docs.aws.amazon.com/lexv2/latest/dg/session-attribs-speech.htm) in the *Amazon Lex Developer Guide*\. 

## Using DTMF inputs during a conversation<a name="bot-dtmf"></a>

Amazon Lex bots support voice and keypad input during a conversation\. The bots interpret keypad input as DTMF digits\. You can prompt contacts to end their input with a pound key \(\#\) and to cancel a conversation by using the star key \(\*\)\. If you don't prompt customers to end their input with the pound key, Lex stops waiting for additional key presses after 5 seconds\.

## Billing and service quotas<a name="bot-billing"></a>

AWS bills you for the following costs:
+ Amazon Chime SDK usage for the call\. For more information, see [Amazon Chime SDK pricing](https://aws.amazon.com/chime/chime-sdk/pricing/)\.
+ Amazon Lex usage for interpreting users' speech\. For more information, see [Amazon Lex streaming conversation pricing](https://aws.amazon.com/lex/pricing/)\.
+ Amazon Polly usage for synthesizing text responses from your bot\. For more information, see the [Amazon Polly Pricing](https://aws.amazon.com/polly/pricing/)\.

You also need to be aware of the following service quotas:
+ Amazon Lex has a service quota for the maximum number of concurrent voice conversations per Lex bot\. You can contact the Amazon Lex service team for quota increases\. For more information, see the Amazon Lex [ Guidelines and quotas](https://docs.aws.amazon.com/lexv2/latest/dg/quotas.html) in the *Amazon Lex Developer Guide*\.
+ Amazon Polly has a service quota for synthesizing text responses\. You can contact the Amazon Polly service team for quota increases\. For more information about Amazon Polly service quotas, see [Quotas in Amazon Polly](http://aws.amazon.com/polly/latest/dg/limits.html)\.