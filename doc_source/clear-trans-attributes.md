# Clearing TransactionAttributes<a name="clear-trans-attributes"></a>

To clear the contents of the `TransactionAttributes` object, pass the `TransactionAttributes` field with an empty JSON Object:

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
    }
}
```

**Note**  
You can't clear data from a `TransactionAttributes` structure by setting its value to `null`\. Also, omitting the `TransactionAttribute` structure doesn't clear its data\. Always pass an empty JSON object with `TransactionAttributes` to clear data from the object\.