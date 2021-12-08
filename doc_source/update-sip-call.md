# Updating in\-progress calls<a name="update-sip-call"></a>

As part of the PSTN Audio Service, SIP media applications allow you to set actions that are executed on a call by invoking user\-defined Lambda functions based on the call events, such as an incoming call or DTMF digits\. The [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API allows you to trigger a Lambda function at any time while a call is active, replacing the current actions with new actions returned by the invocation\.

**Workflow**  
You use the [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API in variety of cases, such as adding participants to a meeting, muting and unmuting user, disconnecting them, and so on\. The following use case describes a typical workflow\.

A user calls and listens to music while Amazon Chime sets up the meeting\. Once setup completes, Amazon Chime stops the audio and admits the caller into the meeting\. Next, assume the use of a separate system, `MyMeetingService`, that manages meetings\. Every incoming call should be put on hold\. Chime notifies MyMeetingService about incoming calls, and MyMeetingService then creates an attendee for each call, and when the MyMeetingService is ready to start the meeting, it notifies the SIP media application and provides a token for joining the meeting\.

To handle this case, the Lambda function has to implement the following logic\. 
+ When a new incoming call arrives, the Lambda is invoked with a `NEW_INBOUND_CALL` event\. The Lambda calls the `MyMeetingService` and passes the `transactionId` that identifies the current call, and returns the `PlayAudio` action\.
+ When the `MyMeetingService` is ready to add the caller to the meeting, the service calls the [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API and passes the call's `transactionId` and `JoinToken` as part of its arguments\. This API call triggers the Lambda function again, now with the `CALL_UPDATE_REQUESTED` event\. The MyMeetingService passes the `JoinToken` to the Lambda function as part of the event, and the token is used to return the `JoinChimeMeeting` action to the SIP media application, which interrupts the `PlayAudio` action and connects the caller to the meeting\.

![\[Diagram showing the flow of data in the UpdateSipMediaApplicationCall API.\]](http://docs.aws.amazon.com/chime/latest/dg/images/update-sip-call-flow3.png)

**Note**  
The [UpdateSipMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_UpdateSipMediaApplicationCall.html) API returns HTTP 202 \(Accepted\)\. The SIP media application confirms that the call is in progress and can be updated, so it attempts to invoke the Lambda function\. The invocation is performed asynchronously, so a successful response from the API doesn’t guarantee that the Lambda function has started or completed\.

The following example shows the request syntax\.

```
{
    "SipMediaApplicationId": "string",
    "TransactionId": "string",
    "Arguments": {
        "string": "string"
    } 
}
```

**Request parameters**
+ **SipMediaApplicationId** – The ID of the SIP media application that handles the call\. 
+ **TransactionId** – The ID of the call transaction\. For inbound calls, the `TransactionId` can be obtained from the `NEW_INCOMING_CALL` event passed to the Lambda function on its first invocation\. For outbound calls, `TransactionId` is returned in the response of [CreateSipMediaApplicationCall](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateSipMediaApplicationCall.html)\. 
+ **Arguments** – Custom arguments made available to the Lambda function as part of the `CallUpdateRequest` action data\. Can contain 0 to 20 key\-value pairs\.

The following example shows a typical request\.

```
aws chime update-sip-media-application-call --sip-media-application-id feb37a7e-2b66-49fb-b2dd-30f4780dc36d --transaction-id 1322a4e7-c106-4e70-aaaf-a8fa4c77c0cb --arguments '{"JoinToken": "abc123"}'
```

**Response syntax**

```
{
  "SipMediaApplicationCall": {
  "TransactionId": "string"
  }
}
```

**Response elements**
+ **TransactionId** – The ID of the call transaction, the same ID as the request\.

The following example shows a `CALL_UPDATE_REQUESTED` invocation event\.

```
{
  "SchemaVersion": "1.0",
  "Sequence": 2,
  "InvocationEventType": "CALL_UPDATE_REQUESTED",
  "ActionData": {
    "Type": "CallUpdateRequest",
    "Parameters": {
      "Arguments": {
        "string": "string"
      }
    }
  },
  "CallDetails": {
    ...
  }
}
```

**Event elements**
+ **SchemaVersion** – The version of the JSON schema \(1\.0\)
+ **Sequence** – The sequence number of the event in the call
+ **InvocationEventType** – The type of Lambda invocation event, in this case, `CALL_UPDATE_REQUESTED`
+ **ActionData** – The data associated with the `CallUpdateRequest` action\.
  + **Type** – The type of action, in this case, `CallUpdateRequest`
  + **Parameters** – The parameters of the action
    + **Arguments** – The arguments passed as part of the `UpdateSipMediaApplicationCall` API request
+ **CallDetails** – The information about the current call state

**Understanding interruptible and non\-interruptable actions**  
When a Lambda function returns a new list of actions while existing actions are being executed, all actions that follow the in\-progress action are replaced with the new actions\. In some cases, the Lambda function interrupts in\-progress actions in order to execute new actions immediately\.

The following diagram shows a typical example\. Text below the digram explains the logic\.

![\[Diagram showing how actions can be replaced during an ongoing SIP media application call.\]](http://docs.aws.amazon.com/chime/latest/dg/images/update-sip-actions.png)

If Action 2 is interruptible, we stop it and execute new Action 1 instead\.

If Action 2 is not interruptible, it completes before the new Action 1 starts\.

In both cases, Action 3 isn't executed\.

If something interrupts an action, the Lambda function is invoked with an `ACTION_INTERRUPTED` event\. This event is used for informational purpose only\. The SIP media application ignores all actions returned by this invocation\.

Types of interruptible actions:
+ `PlayAudio`
+ `RecordAudio`
+ `Pause`

**Sample Lambda function**  
This example shows a typical Lambda function that plays an audio file, passes a join token, and updates the call\.

```
const MMS = require('my-meeting-service');
const myMeetingServiceClient = new MMS.Client();

exports.handler = async (event) => {
    console.log('Request: ' + JSON.stringify(event));
    
    const playAudio = () => {
      return {
        Type: 'PlayAudio',
        Parameters: {
          ParticipantTag: 'LEG-A',
          AudioSource: {
            Type: 'S3',
            BucketName: 'chime-meetings-audio-files-bucket-name',
            Key: 'welcome.wav'
          }
        }
      }
    }
    
    const joinChimeMeeting = (joinToken) => {
      return {
        Type: 'JoinChimeMeeting',
        Parameters: {
          JoinToken: joinToken
        }
      }
    }
    
    const response = (...actions) => {
      const r = {
        SchemaVersion: '1.0',
        Actions: actions
      };
      console.log('Response: ' + JSON.stringify(r));
      return r;
    };
    
    switch (event.InvocationEventType) {
      case 'NEW_INBOUND_CALL': 
        myMeetingServiceClient.addPendingCall(event.CallDetails.TransactionId);         
        return response(playAudio());      
      case 'CALL_UPDATE_REQUESTED':
        const joinToken = event.ActionData.Parameters.Arguments['JoinToken']
        return response(joinChimeMeeting(joinToken));
      default:
        return response();
    }
}
```