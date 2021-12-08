# Subscribing to remote attendee callbacks when an attendee joins<a name="subscribe-remote-attendee"></a>

 When an attendee joins the meeting, we receive a callback to the `attendeePresenceHandler`\. Within this handler we register the volume indicator callbacks for that attendee\. This can drive the mic activity indicator for each attendee in the roster so users know who is speaking at any time\. 

```
attendeePresenceHandler(attendeeId: string, present: boolean): void => {
    this.audioVideo.realtimeSubscribeToVolumeIndicator(
        attendeeId,
        async (
          attendeeId: string,
          volume: number | null,
          muted: boolean | null,
          signalStrength: number | null
        ) => {
            // Handle volume strength changed event
        });
    ...
}
```