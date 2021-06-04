# ReceiveDigits<a name="listen-to-digits"></a>

When a user enters digits or symbols that match the regex pattern specified in an action, the SIP application invokes a Lambda function\.

```
{
    "Type": "ReceiveDigits",
    "Parameters": {
        "CallId": "call-id-1",
        "InputDigitsRegex": "^\d{2}#$",
        "InBetweenDigitsDurationInMilliseconds": 1000, 
        "FlushDigitsDurationInMilliseconds": 10000
    }
}
```

**CallID**  
*Description* – `CallId` of participant in the `CallDetails`\.  
*Allowed values* – A valid call ID\.  
*Required* – No, if `ParticipantTag` is present\.  
*Default value* – None\.

**ParticipantTag**  
*Description* – `ParticipantTag` of one of the connected participants in the `CallDetails`\.  
*Allowed values* – `LEG-A` or `LEG-B`\.  
*Required* – No, if `CallId` is present\.  
*Default value* – `ParticipantTag` of the invoked `callLeg`\. Ignored if you specify `CallId`\.

**InputDigitsRegex**  
*Description* – A regex pattern\.  
*Allowed values* – A valid regular expression pattern\.  
*Required* – Yes\.  
*Default value* – None\.

**InBetweenDigitsDurationInMilliseconds**  
*Description* – Interval between digits before checking to see if the input matches the regex pattern  
*Allowed values* – Duration in milliseconds  
*Required* – Yes  
*Default value* – None

**FlushDigitsDurationInMilliseconds**  
*Description* – Interval after which received digits are flushed or ignored\. If the SIP media application receives a new digit after the interval ends, the timer starts again\.  
*Allowed values* – `InBetweenDigitsDurationInMilliseconds`  
*Required* – Yes  
*Default value* – None

A SIP media application listens for digits for the length of a call unless it receives a new `ReceiveDigits` action\. For every digit received, the application compares it to the regex pattern\. If the initial digits match that pattern, the `InBetweenDigitsDurationInMilliseconds` interval starts\. If the user doesn't enter the remaining necessary digits, the SIP media application invokes the Lambda function for the next set of actions\. If the pattern doesn't match, the application continues listening to digits by storing the previously received digit or digits\.

The `FlushDigitsDurationInMilliseconds` interval starts when the application receives the first digit\. If the application receives a digit after the flush interval ends, it ignores the previous digits and the cycle starts again\. If it successfully receives a set of digits, the SIP media application invokes the Lambda function described in [Capturing digits](case-4.md)\.