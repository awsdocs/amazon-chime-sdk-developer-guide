# Hangup<a name="hangup"></a>

Sends a `Hangup` value with a `SipStatusCode` to any leg of a call\.

When the PSTN Audio service runs a `Hangup` action on a call leg:
+ For a call with only one call leg, the SIP media application invokes the AWS Lambda function with a `HANGUP` event and ignores the response\. The call is then disconnected\.
+ For a call leg \(Leg A\) that is bridged to another call leg \(Leg B\), if the `Hangup` action is associated with the bridged call leg \(Leg B\) then the PSTN audio service disconnects the bridged call leg, then invokes the Lambda function with a `HANGUP` event for leg B\. The PSTN audio service then runs any actions returned from that Lambda invocation\.
+ For a call leg \(Leg A\) that is bridged to another call leg \(Leg B\), if the `Hangup` action is associated with the original call leg \(Leg A\), then the PSTN audio service disconnects the original call leg, then invokes the Lambda function with a `HANGUP` event for leg A\. The PSTN audio service then runs any actions returned from that Lambda invocation\.
+ For a call leg that joined to a meeting using the `JoinMeeting` action, if the `Hangup` action is associated with the meeting leg \(usually Leg B\) then the caller disconnects from the meeting and receives an `ACTION_SUCCESSFUL` event for the `Hangup` action\.

The following example shows a typical `Hangup` action\.

```
{
    "Type": "Hangup",
    "Parameters": {
        "CallId": "call-id-1",
        "ParticipantTag": "LEG-A",
        "SipResponseCode": "0"
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

**SipResponseCode**  
*Description* – Any of the supported SIP response codes  
*Allowed values* – 480–Unavailable; 486–Busy; 0–Normal Termination  
*Required* – No  
*Default value* – 0

After a user ends a call, the SIP media application invokes an AWS Lambda function with the code listed in [Ending a call](case-5.md)\.

See working examples on GitHub:
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-bridging](https://github.com/aws-samples/amazon-chime-sma-bridging)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-call\-forwarding](https://github.com/aws-samples/amazon-chime-sma-call-forwarding)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-outbound\-call\-notifications](https://github.com/aws-samples/amazon-chime-sma-outbound-call-notifications)
+ [https://github\.com/aws\-samples/amazon\-chime\-sma\-on\-demand\-recording](https://github.com/aws-samples/amazon-chime-sma-on-demand-recording)