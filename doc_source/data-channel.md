# Understanding messages in the data\-channel folder<a name="data-channel"></a>

The data\-channel folder contains data messages in the \.txt format, and each message is a JSON object\. Messages are visible with all configurations options\. File names contain the *yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds* timestamp\. This example shows the data fields in a message\.

```
{
    "Timestamp": "string", 
    "Topic": "string", 
    "Data": "string", 
    "SenderAttendeeId": "string"
}
```