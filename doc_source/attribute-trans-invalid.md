# Invalid inputs<a name="attribute-trans-invalid"></a>

The following example shows an invalid input\. In this case, the JSON object passes too many items to a SIP media application\.

```
{ 
    "SchemaVersion": "1.0", 
    "Actions": [ 
        { 
            "Type": "PlayAudio", 
            "Parameters": { 
                "ParticipantTag": "LEG-A", 
                "AudioSource": { 
                    "Type": "S3", 
                    "BucketName": "mtg1-sipmedia-app-iad", 
                    "Key": "Welcome3.wav" 
                } 
            } 
        } 
    ], 
    "TransactionAttributes": { 
        "key1": "value1", 
        "key2": "value2", 
        "key3": "value3", 
        "key4": "value4", 
        "key5": "value5", 
        "key6": "value6", 
        "key7": "value7", 
        "key8": "value8", 
        "key9": "value9", 
        "key10": "value10", 
        "key11": "value11" 
    } 
}
```

The following example shows the response to the previously given input\. This output is passed from a SIP media application back to the AWS Lambda function that invoked the application\.

```
{ 
    "SchemaVersion": "1.0", 
    "Sequence": 2, 
    "InvocationEventType": "INVALID_LAMBDA_RESPONSE", 
    "CallDetails": { 
        "TransactionId": "mtg1-tx-id", 
        "AwsAccountId": "166971021612", 
        "AwsRegion": "us-east-1", 
        "SipRuleId": "aafbd402-b7a2-4992-92f8-496b4563c492", 
        "SipMediaApplicationId": "e88f4e49-dd21-4a3f-b538-bc84eae11505", 
        "Participants": [ 
            { 
                "CallId": "72cbec69-f098-45d8-9ad6-e26cb9af663a", 
                "ParticipantTag": "LEG-A", 
                "To": "+14345550101", 
                "From": "+14255550199", 
                "Direction": "Inbound", 
                "StartTimeInMilliseconds": "1644540839987" 
            } 
        ] 
    }, 
    "ErrorType": "TransactionAttributesInvalidMapSize", 
    "ErrorMessage": "Transaction Attributes has too many mappings. Maximum number of mappings is 10" 
}
```