# ReceiveDigits<a name="listen-to-digits"></a>

When a user enters digits that match the regular expression pattern specified in this action, the SIP media application invokes the AWS Lambda function\.

```
{
    "Type": "ReceiveDigits",
    "Parameters": {
        "CallId": "call-id-1",
        "ParticipantTag": "LEG-A",
        "InputDigitsRegex": "^\d{2}#$",
        "InBetweenDigitsDurationInMilliseconds": 1000, 
        "FlushDigitsDurationInMilliseconds": 10000
    }
}
```

**CallId**  
*Description* – `CallId` of participant in the `CallDetails` of the AWS Lambda function invocation  
*Allowed values* – A valid call ID  
*Required* – No  
*Default value* – None

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`  
*Allowed values* – `LEG-A` or `LEG-B`  
*Required* – No  
*Default value* – `ParticipantTag` of the invoked `callLeg` Ignored if you specify `CallId`

**InputDigitsRegex**  
*Description* – A regular expression pattern  
*Allowed values* – A valid regular expression pattern  
*Required* – Yes  
*Default value* – None

**InBetweenDigitsDurationInMilliseconds**  
*Description* – Interval between digits before checking to see if the input matches the regular expression pattern  
*Allowed values* – Duration in milliseconds  
*Required* – Yes  
*Default value* – None

**FlushDigitsDurationInMilliseconds**  
*Description* – Interval after which received DTMF digits are flushed and sent to the AWS Lambda function\. If the SIP media application receives a new digit after the interval ends, the timer starts again\.  
*Allowed values* – `InBetweenDigitsDurationInMilliseconds`  
*Required* – Yes  
*Default value* – None

The SIP media application discards DTMF digits for the duration of a call until it receives a new `ReceiveDigits` action\. The `FlushDigitsDurationInMilliseconds` interval starts when the SIP media application receives the first DTMF digit\. If the user enters the correct digits before the interval expires, the SIP media application invokes the AWS Lambda function described in [Receiving caller input](case-4.md)\.

If the user input doesn't match the regular expression pattern, the SIP media application repeats the "failure" audio file message until the application exhausts the repeat count or the user inputs valid digits\. 

See working examples on GitHub:
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-outbound\-call\-notifications](https://github.com/aws-samples/amazon-chime-sma-outbound-call-notifications)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-on\-demand\-recording](https://github.com/aws-samples/amazon-chime-sma-on-demand-recording)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-update\-call](https://github.com/aws-samples/amazon-chime-sma-update-call)