# Handling ACTION\_SUCCESSFUL events<a name="attribute-trans-success"></a>

The following example shows how a successful [PlayAudio](play-audio.md) sends the stored `TransactionAttributes` as part of the `CallDetails `\.

```
{ 
    "SchemaVersion": "1.0", 
    "Sequence": 2, 
    "InvocationEventType": "ACTION_SUCCESSFUL", 
    "ActionData": { 
        "Type": "PlayAudio", 
        "Parameters": { 
            "AudioSource": { 
                "Type": "S3", 
                "BucketName": "mtg1-sipmedia-app-iad", 
                "Key": "Welcome3.wav" 
            }, 
            "Repeat": 1, 
            "ParticipantTag": "LEG-A" 
        } 
    }, 
    "CallDetails": { 
        "TransactionId": "mtg1-tx-id", 
        "TransactionAttributes": { 
            "key1": "value1", 
            "key2": "value2" 
        }, 
        "AwsAccountId": "166971021612", 
        "AwsRegion": "us-east-1", 
        "SipRuleId": "aafbd402-b7a2-4992-92f8-496b4563c492", 
        "SipMediaApplicationId": "e88f4e49-dd21-4a3f-b538-bc84eae11505", 
        "Participants": [ 
            { 
                "CallId": "bbff30c5-866a-41b5-8d0a-5d23d5e19f3e", 
                "ParticipantTag": "LEG-A", 
                "To": "+14345550101", 
                "From": "+14255550199", 
                "Direction": "Inbound", 
                "StartTimeInMilliseconds": "1644539405907", 
                "Status": "Connected" 
            } 
        ] 
    } 
}
```