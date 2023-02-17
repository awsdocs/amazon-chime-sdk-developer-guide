# SendDigits<a name="send-digits"></a>

Send up to 50 dual tone multi\-frequency \(DTMF\) digits on any leg of a call\. The signals can include the following:
+ Numbers 0 thru 9
+ Special characters star \(\*\) and pound \(\#\)
+ Network control signals A, B, C, D
+ The comma character \(,\)\. This signal adds a 0\.5 second delay between the previous and next signals\.

**Topics**
+ [Using the SendDigits action](#send-digits-action)
+ [Handling ACTION\_SUCCESSFUL events](#send-digit-success)
+ [Handling ACTION\_FAILED events](#send-digit-fail)
+ [Call flow](#send-digits-call-flow)

## Using the SendDigits action<a name="send-digits-action"></a>

The following example shows a typical `SendDigits` action:

```
{
    "SchemaVersion": "1.0",
    "Actions":[
        {
            "Type": "SendDigits",
            "Parameters": {
                "CallId": "call-id-1", // required
                "Digits": ",,*1234,56,7890ABCD#", // required
                "ToneDurationInMilliseconds": 100 // optional
            }
        }
    ]
}
```

**CallId**  
*Description* – The `CallId` of a participant in the `CallDetails` of the AWS Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – Yes  
*Default value* – None

**Digits**  
*Description* – The digits to be sent on the call leg that corresponds to the `CallId`  
*Allowed values* – 0\-9, \*, \#, A, B, C, D, comma \(,\)  
*Required* – Yes  
*Default value* – None

**ToneDurationInMilliseconds**  
*Description* – The amount of time allowed, in milliseconds, to transmit each digit\.  
*Allowed values* – Any integer between 50 and 24000  
*Required* – No  
*Default value* – 250

## Handling ACTION\_SUCCESSFUL events<a name="send-digit-success"></a>

The following example shows a typical `ACTION_SUCCESSFUL` event for the `SendDigits` action\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_SUCCESSFUL",
    "ActionData": {
        "Type": "SendDigits",
        "Parameters": {
            "Digits": "1,2A#",
            "ToneDurationInMilliseconds": 100,
            "CallId": "call-id-1"
        },  
    "CallDetails": { 
        ...
        }
    }
}
```

## Handling ACTION\_FAILED events<a name="send-digit-fail"></a>

The following example shows a typical `ACTION_FAILED` event for the `SendDigits` action\.

```
{
    "SchemaVersion": "1.0",
    "Sequence": 3,
    "InvocationEventType": "ACTION_FAILED",
    "ActionData": {
        "Type": "SendDigits",
        "Parameters": {
            "Digits": "1,2A#",
            "ToneDurationInMilliseconds": 20000000,
            "CallId": "call-id-1"
        },
    "ErrorType": "InvalidActionParameter",
    "ErrorMessage": "ToneDuration parameter value is invalid."
    },
    "CallDetails": {
        ...
        }
    }
}
```

## Call flow<a name="send-digits-call-flow"></a>

The following diagram shows the program flow for sending digits from a caller to a callee\.

![\[Diagram showing the program flow of the SendDigits action.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/send-digits-1.png)