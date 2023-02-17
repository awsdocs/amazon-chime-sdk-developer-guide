# VoiceFocus<a name="voice-focus"></a>

Enables you to apply Amazon Voice Focus noise suppression to inbound and outbound call legs on a public switched telephony network \(PSTN\) call\. When you apply Amazon Voice Focus, it reduces background noise without impacting human speech\. This can make the current speaker easier to hear\.

To create inbound call legs, you use a [SIP rule](https://docs.aws.amazon.com/chime/latest/ag/manage-sip-applications.html) that invokes an AWS Lambda function with a `NewInboundCall` event\. You can create outbound call legs by using the [CallAndBridge](call-and-bridge.md) action, or by using a [CreateSIPMediaApplicationCall](chime/latest/APIReference/API_CreateSipMediaApplicationCall.html) API operation\. For more information about Amazon Voice Focus, see [ How the Amazon Chime SDK's noise cancellation works](https://www.amazon.science/blog/how-amazon-chimes-challenge-winning-noise-cancellation-works)\.

 Amazon Voice Focus reduces unwanted, non\-speech noises, including: 
+ **Environment noises **—wind, fans, running water
+ **Background noises**—lawnmowers, barking dogs
+ **Foreground noises**—typing, paper shuffling

**Note**  
When you use Amazon Voice Focus, AWS bills you for the active call minutes of each call leg and for each minute of SIP media application usage\.

This example shows a typical `VoiceFocus` action\.

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "VoiceFocus",
            "Parameters": {
                "Enable": True|False,            // required
                "CallId": "call-id-1",           // required    
            }
        }
    ]
}
```

**Enable**  
*Description* – Enables or disables Amazon Voice Focus  
*Allowed values* – `True` \| `False`  
*Required* – Yes  
*Default value* – None

**CallId**  
*Description* – CallId of participant in the `CallDetails` of the AWS Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – Yes  
*Default value* – None

This example shows a successful `ACTION_SUCCESSFUL` event for the `VoiceFocus` action\.

```
{
   "SchemaVersion": "1.0",
   "Sequence": 3,
   "InvocationEventType": "ACTION_SUCCESSFUL",
   "ActionData": {
      "Type": "VoiceFocus",
      "Parameters": {
         "Enable": True,
         "CallId": "call-id-1"
      }
   },
   "CallDetails":{
      .....
      .....
      "Participants":[
         {
            "CallId": "call-id-of-caller",
            .....   
            "Status": "Connected"
         },
         {
            "CallId": "call-id-of-callee",
            .....
            "Status": "Connected"
         }
      ]
   }
}
```

This example shows a typical `ACTION_FAILED` event for the `VoiceFocus` action\.

```
{
   "SchemaVersion": "1.0",
   "Sequence":2,
   "InvocationEventType": "ACTION_FAILED",
      "ActionData":{
      "Type": "VoiceFocus",
      "Parameters": {
         "Enable": True,
         "CallId": "call-id-1"
      }
      },
      "ErrorType": "SystemException",
      "ErrorMessage": "System error while running action"
   },
   "CallDetails":{
      .....
      .....
      "Participants":[
         {
            "CallId": "call-id-of-caller",
            .....   
         }
      ]
   }
}
```

**Error handling**  
For security reasons, the PSTN Audio actions have a limit of 5 call requests per second, per customer account \(CPS\)\. When call requests exceed the 5 CPS limit, the action returns an error message\. This table lists the error messages returned by the `VoiceFocus` action\.


| Error | Message | Reason | 
| --- | --- | --- | 
| `ActionExecutionThrottled` | Failed to run action\. The maximum number of actions per second has been reached\. | The number of Voice Focus action requests per second exceeded the system limit\.  | 
| `MissingRequiredActionParameter` | Missing required action parameter\. | Missing one or more of the required parameters while running the action\. | 
| `SystemException` | System error while running action\. | A system error occurred while running the action\. | 

**Call flows**  
This diagram shows the call flow for enabling and disabling Amazon Voice Focus for a `CallAndBridge` action between two PSTN calls\.

![\[The call flow when you enable or disable Amazon Voice focus for two bridged PSTN calls. For the outbound call leg, the AWS Lambda function enables Amazon Voice focus for the caller and returns a set of actions, including CallAndBridge. Once the call is bridged, the VoiceFocus action returns an ACTION_SUCCESSFUL event, and the Lambda function returns another set of events that enables Amazon Voice Focus for the person being called. That set of actions includes VoiceFocus, Enable, True, and the caller's ID. No further action is taken until the caller hangs up. The Lambda function then sends a Hangup action to the SIP media application. The application hangs up the person being called and sends a Hangup function back to the Lambda function, which takes no further actions.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/voice_focus-pstn1.png)