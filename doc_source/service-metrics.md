# Service metrics<a name="service-metrics"></a>

The Amazon Chime SDK publishes to the following service metrics to the `AWS/ChimeSDK` namespace:


| Metric | Unit | Description | 
| --- | --- | --- | 
| `AttendeeAuthorizationSuccess` | Count | Total count of successful authorization attempts\. Success means that an attendee was allowed to join the meeting\. | 
| `AttendeeAuthorizationError` | Count | Total count of authorization failures, indicates that the attendee couldn't join the meeting\. | 
| `AttendeeAudioDrops` | Count | Total count of audio drops\. | 
| `AttendeeContentDrops` | Count | Total count of content share drops\. | 
| `MeetingSQSNotificationErrors` | Count | Total count of SQS Notification errors\. | 
| `MeetingSNSNotificationErrors` | Count | Total count of SNS Notification errors\. | 