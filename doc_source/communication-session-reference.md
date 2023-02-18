# StartCommunicationSession API reference<a name="communication-session-reference"></a>

The following reference provides the details of the `StartCommunicationSession` API\. Amazon Alexa provides the API\.

## StartCommunicationSession<a name="start-communication-session"></a>

Starts a communication session between a participant that originates the session and a participant that receives the session\.

### Request Syntax<a name="start-session-request"></a>

```
POST /v1/communications/session HTTP/1.1 
Authorization: Bearer AuthorizationToken
Content-type: application/json

{
  "participants":[
    {
      "id": {
        "type": "PHONE_NUMBER",
        "value": "string"
      },
      "endpointId": "string",
      "isOriginator": true
    },
    {
      "id": {
        "type": "PHONE_NUMBER",
        "value": "string"
      },
      "communicationProviderId": "amzn1.alexa.csp.id.82bb98bc-384a-11ed-a261-0242ac120002"
    }
  ],
  "clientContext": {
    "clientSessionId": "string"
  }
}
```

#### URI Request Parameters<a name="start-session-uri"></a>

**AuthorizationToken**  
The authorization token for this request\.  
Type: string  
Required: Yes

#### Request Body<a name="start-session-req-body"></a>

The request accepts the following data in JSON format\.

**participants**  
The participants in the communication session\. One of the participants must be the originator of the session\. The other participant must be a non originator\.  
Type: [Participant](#dt-participant) object  
Length Constraints: Minimum length of 2\. Maximum length of 2\.  
Required: Yes

**id**  
The identifier of this participant\.  
Type: [ParticipantId](#dt-participant-id) object  
Required: Yes

**clientContext**  
The client context data for the communication session\.  
Type: [ClientContext](#dt-client-context) object  
Required: No

#### Response Syntax<a name="start-session-response-syntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "sessionId": "string"
}
```

#### Response Elements<a name="start-session-response"></a>

If the request succeeds, the service sends back an HTTP 200 response\. The service returns the following data in JSON format\.

**sessionId**  
The communication session's identifier  
Type: string

#### Error Response Syntax<a name="start-session-error-syntax"></a>

```
HTTP/1.1 StatusCode
Content-type: application/json
{
   "code": "string",
   "message": "string"
}
```

#### Error Response Elements<a name="start-session-error-elements"></a>

If requests fail, the service sends back an HTTP 4XX response for client\-side errors and an HTTP 5XX response for server\-side errors\. The service returns the following data in JSON format\.

**code**  
The failed request's error code\.  
Type: string

**message**  
The failed request's error message\.  
Type: string

#### Errors<a name="start-session-errors"></a>

**InvalidInput**  
The request is malformed, is missing any required parameters, or does not match the service's restrictions\.   
HTTP Status Code: 400

**BadToken**  
The authorization token is expired or invalid\.  
HTTP Status Code: 401

**AccessDenied**  
The authorization token does not have the required permission to access the service\.  
HTTP Status Code: 403

**Throttling**  
The client exceeded its request rate limit\.  
HTTP Status Code: 429

**InternalServerError**  
The service encountered an unexpected error\.  
HTTP Status Code: 500

### Data types<a name="start-comm-session-data-types"></a>

The `StartCommunicationSession` API supports the following data types\.

#### ClientContext<a name="dt-client-context"></a>

The client context data of a communication session\. 

**clientSessionId**  
The client provided session ID for a communication session\. The `clientSessionId` is provided in the [User\-to\-User SIP header](https://datatracker.ietf.org/doc/html/rfc7433) in the SIP INVITE\. You should use a unique identifier for each communication session\. For more information, see [Using clientSessionId to send call context data](call-context-data.md)\.  
Type: string  
Length constraints: Minimum length of 1\. Maximum length of 100  
Required: No

#### Participant<a name="dt-participant"></a>

A participant in the communication session\.

**id**  
The identifier of this participant\.  
Type: `PHONE_NUMBER`  
Value: Phone number in the E\.164 format  
Required: Yes

**endpointId**  
The endpoint ID of this participant\. The endpoint ID is required if this participant originates the communication session\.  
Type: string  
Length Constraints: Minimum length of 1\. Maximum length of 512  
Required: No

**isOriginator**  
The originator of the communication session\.  
Type: Boolean  
Required: No

**communicationProviderId**  
The ID of the communication service provider used to establish a communication session with the participant\. The `communicationProviderId` is required if this participant is not the session originator\. The value of the `communicationProviderId` must be `amzn1.alexa.csp.id.82bb98bc-384a-11ed-a261-0242ac120002`\.  
Type: string  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Required: No

#### ParticipantId<a name="dt-participant-id"></a>

The identifier of a participant\. 

**type**  
The type of the participant ID\.   
Type: string  
Valid Values: `PHONE_NUMBER`  
Required: Yes

**value**  
The value of the participant ID\. Must be in E\.164 phone number format\.  
Type: string  
Length Constraints: Minimum length of 1\. Maximum length of 128  
Required: Yes