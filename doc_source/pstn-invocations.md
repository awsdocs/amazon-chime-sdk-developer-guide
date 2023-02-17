# Understanding telephony events<a name="pstn-invocations"></a>

The PSTN Audio service invokes your AWS AWS Lambda function when certain events occur during a call\. The following example shows the events, and text after the example explains each event\.

```
{ 
    "SchemaVersion": "1.0", 
    "Sequence": 3, 
    "InvocationEventType": "event-type", 
    "CallDetails": { 
        "TransactionId": "transaction-id-1", 
        "AwsAccountId": "aws-acct-id-1", 
        "AwsRegion": "us-east-1", 
        "SipMediaApplicationId": "sip-media-app-id-1", 
        "Participants": [ 
            { 
                "CallId": "call-id-1", 
                "ParticipantTag": "LEG-A", 
                "To": "e164PhoneNumber", 
                "From": "e164PhoneNumber", 
                "Direction": "Inbound/Outbound", 
                "StartTimeInMilliseconds": "1641998241509", 
                "Status": "Connected/Disconnected" 
            } 
        ] 
    } 
}
```

**SchemaVersion**  
The version of schema used to create this event object\.

**Sequence**  
The sequence of events that invoke your AWS Lambda function\. Each time your function is invoked during a call, the sequence is incremented\.

**InvocationEventType**  
The type of event that triggers an AWS Lambda invocation\. For more information, see [Event types](#pstn-event-types) later in this topic\.

**CallDetails**  
Information about the call associated with the AWS Lambda invocation\.

**TransactionId**  
The ID of a call associated with an AWS Lambda invocation\.

**AwsAccountId**  
The AWS account ID associated with the SIP media application that resulted in the call routing\.

**SipMediaApplicationId**  
The ID of the SIP media application associated with the call\.

**Participants**  
Information about the participants on the call that invokes an AWS AWS Lambda function\.

**CallId**  
A unique ID assigned to each participant\.

**ParticipantTag**  
Each call participant gets a tag, `LEG-A` or `LEG-B`\.

**To**  
The participant "to" phone number, in E\.164 format\.

**From**  
The participant “from” phone number, in E\.164 format\.

**Direction**  
The direction that a call leg comes from\. `Inbound` represents a call made to the PSTN Audio service\. `Outbound` represents a call made from the PSTN Audio service\.

**StartTimeInMilliseconds**  
The epoch time in milliseconds, starting when a participant joins a call\.

**Status**  
Whether a participant is `Connected` or `Disconnected`

## Event types<a name="pstn-event-types"></a>

The PSTN Audio service invokes the Lambda function with these event types:

**NEW\_INBOUND\_CALL**  
A new call has been initiated by a phone number associated with your SIP media application\.

**NEW\_OUTBOUND\_CALL**  
A new outbound call has been made via the [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateSipMediaApplicationCall.html) API\.

**ACTION\_SUCCESSFUL**  
An action returned from your AWS Lambda function has succeeded\. Successful actions include `ActionData` that matches the successful action\.   

```
    "ActionData": {
        // The previous successful action 
    },
```

**ACTION\_FAILED**  
An action returned from your AWS Lambda function did not succeed\. Unsuccessful actions include `ActionData` that matches the failed action, an error type, and an error message that describes the failure:  

```
    "ActionData": {
        // The previous unsuccessful action
        "ErrorType": "error-type",
        "ErrorMessage": "error message"
    },
```

**ACTION\_INTERRUPTED**  
An action in the process of running was interrupted by an [ UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API invocation\. The `ActionData` includes the interrupted actions:   

```
"ActionData": {
        // The action that was interrupted
    },
```

**HANGUP**  
A user or the application hung up a call leg\. The `ActionData` includes these details about the event:  

```
   "ActionData": {
        "Type": "Hangup",
        "Parameters": {
            "SipResponseCode": 486,
            "CallId": "c70f341a-adde-4406-9dea-1e01d34d033d",
            "ParticipantTag": "LEG-A"
        }
    },
```  
**Type**  
Hangup\.  
**Parameters**  
The information about the `HANGUP` event:   
+ **SipResponseCode** – The response code associated with the event\. The most common codes are:
  + **0** – Normal clearing
  + **480** – No answer
  + **486** – User busy
+ **CallId** The ID of the participant that hung up\.
+ **ParticipantTag** The tag of the participant that hung up\.

**CALL\_ANSWERED**  
The PSTN Audio service answered an incoming call was answered\. This event is returned on a dial\-out call unless the call is bridged\.

**INVALID\_LAMBDA\_RESPONSE**  
The response provided to the last AWS Lambda invocation caused a problem\. The `ActionData` includes these additional fields:  

```
    "ErrorType": "error-type-1", 
    "ErrorMessage": "error-msg-1"
```

**DIGITS\_RECEIVED**  
The application received DTMF digits after completion of a `ReceiveDigits` action\. The `ActionData` includes the received digits\.  

```
    "ActionData": {
        "ReceivedDigits": ###
        // The ReceiveDigits action data
    },
```

**CALL\_UPDATE\_REQUESTED**  
The [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API was invoked\. The `ActionData` includes information about the update request:  

```
    "ActionData": {
        "Type": "CallUpdateRequest", 
        "Parameters": {
            "Arguments": {
                "leg": "LEG-A"
                }
            }
        },
    }
```

**RINGING**  
A call leg is ringing