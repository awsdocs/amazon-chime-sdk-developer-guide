# CallAndBridge<a name="call-and-bridge"></a>

Enables you to create an outbound call to a PSTN phone number and bridge it with an existing call leg\. An existing call leg can be an inbound leg created by a SIP rule that invokes the AWS Lambda function with a `NewInboundCall` event, or an outbound call leg created by using the [CreateSIPMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateSipMediaApplicationCall.html) API\. The `CallAndBridge` action only supports calling and bridging to a PSTN endpoint\. 

The following example code shows a typical action\.

```
{
   "SchemaVersion": "1.0",
   "Actions":[
      {
         "Type": "CallAndBridge",
         "Parameters":{
            "CallTimeoutSeconds": 30,
            "CallerIdNumber": "e164PhoneNumber", // required
            "RingbackTone": { // optional
                    "Type": "S3",
                    "BucketName": "bucket-name",
                    "Key": "audio-file.wav"
                },
            "Endpoints":[
               {
                  "Uri": "e164PhoneNumber", // required
                  "BridgeEndpointType": "PSTN" // required
               }
            ]
         }
      }
   ]
}
```

**CallTimeoutSeconds**  
*Description* – The interval before a call times out\. The timer starts at call setup  
*Allowed values* – Integer 0 < x <= 120  
*Required* – No  
*Default value* – 30

**CallerIdNumber**  
*Description* – A number belonging to the customer, or the From number of the A Leg  
*Allowed values* – A valid phone number in the E\.164 format  
*Required* – Yes  
*Default value* – None

**RingbackTone**  
*Description* – Plays the WAV file in the S3 bucket over the A leg\. Plays the file in a loop until the B Leg is answered, the call is rejected, the call timeout limit reached, or the A leg is cancelled  
*Allowed values* – A valid S3 bucket name and WAV file\. For information about the file format, see [PlayAudio](play-audio.md)  
*Required* – No  
*Default value* – None

**Endpoints**  
*Description* – The endpoints of a call  
*Allowed values* – A valid phone number in the E\.164 format  
*Required* – Yes  
*Default value* – None

The following example shows a successful `CallAndBridge` action:

```
{
   "SchemaVersion": "1.0",
   "Sequence": 3,
   "InvocationEventType": "ACTION_SUCCESSFUL",
   "ActionData": {
      "Type": "CallAndBridge",
      "Parameters": {
         "CallTimeoutSeconds": 30,
         "CallerIdNumber": "e164PhoneNumber",
         "Endpoints":[
            {
               "Uri": "e164PhoneNumber",
               "BridgeEndpointType": "PSTN"
            }
         ],
         "CallId": "call-id-1"
      }
   },
   "CallDetails":{
      .....
      .....
      "Participants":[
         {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            .....   
            "Status": "Connected"
         },
         {
            "CallId": "call-id-2",
            "ParticipantTag": "LEG-B",
            .....
            "Status": "Connected"
         }
      ]
   }
}
```

The following example shows a failed `CallAndBridge` action\.

```
{
   "SchemaVersion": "1.0",
   "Sequence":2,
   "InvocationEventType": "ACTION_FAILED",
      "ActionData":{
      "Type": "CallAndBridge",
      "Parameters":{
         "CallTimeoutSeconds": 30,
         "CallerIdNumber": "e164PhoneNumber",
         "Endpoints": [
            {
               "Uri": "e164PhoneNumber",
               "BridgeEndpointType": "PSTN"
            }
         ],
         "CallId": "call-id-1"
      },
      "ErrorType": "CallNotAnswered",
      "ErrorMessage": "Call not answered"
   },
   "CallDetails":{
      .....
      .....
      "Participants":[
         {
            "CallId": "call-id-1",
            "ParticipantTag": "LEG-A",
            .....   
         }
      ]
   }
}
```

**Call flows**  
The `CallAndBridge` action exhibits different call signaling and ringback behaviors for the incoming call leg \(the A leg\) depending whether that leg is answered\. The following sequence diagrams show the behaviors\.

![\[Diagram of basic call flow through the CallAndBridge action.\]](http://docs.aws.amazon.com/chime/latest/dg/images/call-bridge-ans.png)

The following diagram shows the call flow for an unanswered call\.

![\[Diagram of basic call flow through a SIP application and Lambda functions.\]](http://docs.aws.amazon.com/chime/latest/dg/images/SMA_Bridging_NotAns.png)

**Additional Details**  
Remember these facts about the `CallAndBridge` action\.
+ **CallTimeoutSeconds** – This timer starts when the SIP invitation is sent on the B\-Leg\. You can set a desired target value, but this value can be ignored by upstream carriers\.
+ **CallerIdNumber** – This phone number must belong to the customer, or be the From number of an A\-Leg\.
+ **Hang\-up behavior and edge cases** – If one call leg hangs up, the other call leg does not automatically hang up the call\. When a `Hangup` event is sent to the Lambda function, the remaining leg must be disconnected independently\. If a call leg is left hanging, the call is billed until it is hung up\. For example, the following scenario may lead to unexpected charges:
  + You try to bridge to a destination phone number\. The destination is busy and sends the call straight to voicemail\. From the SIP media application's perspective, going to voicemail is an answered call\. The A\-Leg hangs up, but the B\-Leg continues listening for the voicemail message\. While the B\-Leg listens, you get billed\.
  + As a best practice, hang up each call leg independently by using the Lambda function, or by the party on the other end of the call\.
+ **Billing** – You're billed for the following when using `CallAndBridge`:
  + Active call minutes for each call leg created \(A\-Leg, B\-Leg, etc\.\) to the PSTN\.
  + SIP media application usage minutes\.