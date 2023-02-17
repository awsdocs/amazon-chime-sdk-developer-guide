# Understanding meeting event files<a name="meeting-events"></a>

The meeting\-events folder contains meeting events in the \.txt format, and each event is a JSON object\. Messages are visible with all configurations options\. File names contain the <yyyy\-mm\-dd\-hour\-min\-seconds\-milleseconds> timestamp\. This example shows the fields and data in a typical event file\.

```
{
    "Timestamp": "string",
    "EventType": "AttendeeJoined | AttendeeLeft | AttendeeVideoJoined | AttendeeVideoLeft | ActiveSpeaker | CaptureStarted | CaptureEnded  | AudioTrackMute | AudioTrackUnmute",
    "EventParameters": {
        # ...
    }
}
```