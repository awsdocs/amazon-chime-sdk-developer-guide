# Transcription messages<a name="process-msgs"></a>

The Amazon Chime service shares transcription information with attendees by sending `TranscriptEvent` objects in data messages\. A `TranscriptEvent` delivers a `Transcript` or a `TranscriptionStatus`\. 

A `Transcript` includes results with timestamped, user\-attributed words and punctuation\. A result may be “partial”, in which case the system usually updates it in a subsequent `TranscriptEvent`\. This allows you to see transcriptions quickly and apply inline updates later as necessary\.

A `TranscriptStatus` may deliver one of the `TranscriptionStatusType` events, listed in the example in the next section\.

Newer versions of the Amazon Chime SDKs include additional data types and helper functions for common processing a `TranscriptEvent`\.

## TranscriptEvent<a name="transcript-event"></a>

This example shows a typical transcription event\.

```
type TranscriptEvent = Transcript | TranscriptionStatus;

export class TranscriptEventConverter {
  static from(dataMessage: DataMessage): TranscriptEvent[] {
    // convert DataMessage to TranscriptEvents
    return ...
  }
}

export default class TranscriptionStatus {
    type: TranscriptionStatusType;
    eventTimeMs: number;
    transcriptionRegion: string;
    transcriptionConfiguration: string;
    message?: string;
}

enum TranscriptionStatusType {
    STARTED        =    'started',
    INTERRUPTED    =    'interrupted',
    RESUMED        =    'resumed',
    STOPPED        =    'stopped',
    FAILED         =    'failed',
}

export default class Transcript {
    results: TranscriptResult[];    // at least one
}

export class TranscriptResult {
    resultId:        string;
    isPartial:       boolean;
    startTimeMs:     number;
    endTimeMs:       number;
    alternatives:    TranscriptAlternative[];    // most confident first
}

export default class TranscriptAlternative {
    items: TranscriptItem[];    // in start time order
    transcript: string; //concatenated transcript items
}

export default class TranscriptItem {
    type:                      TranscriptItemType;
    startTimeMs:               number;
    endTimeMs:                 number;
    attendee:                  Attendee;
    content:                   string;
    vocabularyFilterMatch?:    boolean;
}

enum TranscriptItemType {
    PRONUNCIATION    =    'pronunciation',// content is a word
    PUNCTUATION      =    'punctuation',// content is punctuation
}

// This is an existing SDK model
export default class Attendee {
    attendeeId:        string;
    externalUserId:    string;
}
```

## Data guidelines<a name="data-guidelines"></a>

Keep these guidelines in mind as you go\.

1. `transcription.results` may have more than one result\.

1. If `transcription.results[i].isPartial = true`, then there may be an update for the entire result\. The update is likely, but not guaranteed\. The update has the same `transcript.result[i].resultId`\. If you want to avoid low\-confidence transcriptions, you can skip partial results completely\. If you want low\-latency results, you can display partial results, then overwrite completely when the update arrives\.

1. `transcription.results[i].alternatives` always contains at least one entry\. If it contains more than one entry, the most confident entry is first in the list\. In most cases, you can take the first entry in `transcription.results[i].alternatives` and ignore the others\.

1. `transcription.results[i].alternatives[j].items` includes an entry for each word or punctuation mark\.

1. `transcription.results[i].alternatives[j].items[k].` content is what was spoken\.

1. `transcription.results[i].alternatives[j].items[k].attendee` is the user attribution \(who\) of the content\.

1. `transcription.results[i].alternatives[j].items[k].startTimeMs` is the "when" of the content\. This enables word\-by\-word rendering of user\-attributed transcription across different users in the order that the words were spoken\.

1. `The transcription.results[i].alternatives[j].items[k].endTimeMs` field can generally be ignored, but is supplied for completeness of who said what when\.

1. `transcription.results[i].alternatives[j].items[k].vocabularyFilterMatch` is true if the content matched a word in the filter, otherwise it is false\.

## Registering event handlers for TranscriptEvents<a name="register-handler"></a>

The following examples use the Amazon Chime SDK for Javascript\. However, the pattern is consistent across all Amazon Chime SDKs\.

The `TranscriptionController` in the `RealtimeController` and `RealtimeControllerFacade` includes specific functions to add a handler for processing `TranscriptionEvents`:

```
/** 
 * Returns the [[TranscriptionController]] for this realtime controller. 
 */
readonly transcriptionController?: TranscriptionController;
```

The `TranscriptionController` has two functions to manage subscribing and unsubscribing to `TranscriptionEvent` callbacks:

```
import TranscriptEvent from './TranscriptEvent';

export default interface TranscriptionController {
  /**
   * Subscribe a callback to handle received transcript event
   */
  subscribeToTranscriptEvent(callback: (transcriptEvent: TranscriptEvent) => void): void;

  /** 
   * Unsubscribe a callback from receiving transcript event 
   */
  unsubscribeFromTranscriptEvent(callback: (transcriptEvent: TranscriptEvent) => void): void;
}
```

**Using the optional `TranscriptionController`**  
We provide a default implementation of `TranscriptionController` interface named `DefaultTranscriptionController`\. The default implementation in `DefaultRealtimeController` and `DefaultAudioVideoFacade` returns a `DefaultTranscriptionController` object:

```
/** 
get transcriptionController(): TranscriptionController {
   return this.realtimeController.transcriptionController;
}
```

`DefaultRealtimeController` also takes an optional `TranscriptionController` object in its constructor\. That allows you to override the `DefaultTranscriptionController` behavior as needed\. Developer applications subscribe and unsubscribe to one or more callbacks through the `TranscriptionController` object of the `AudioVideoFacade` object:

```
// Subscribe
this.audioVideo.transcriptionController?.subscribeToTranscriptEvent(this.transcriptEventHandler);

// Unsubscribe
this.audioVideo.transcriptionController?.unsubscribeFromTranscriptEvent(this.transcriptEventHandler););
```

We provide a default implementation of the `TranscriptionController` interface named `DefaultTranscriptionController`\. The default implementation in `DefaultRealtimeController `and `DefaultAudioVideoFacade` returns a `DefaultTranscriptionController` object:

```
get transcriptionController(): TranscriptionController {
    return this.realtimeController.transcriptionController;
}
```

`DefaultRealtimeController` also takes an optional `TranscriptionController` object in its constructor\. This allows you to override the `DefaultTranscriptionController` behavior\. Your applications can subscribe or unsubscribe to one or more callbacks through the `TranscriptionController` object of the `AudioVideoFacade` object:

```
// Subscribe
this.audioVideo.transcriptionController?.subscribeToTranscriptEvent(this.transcriptEventHandler);

// Unsubscribe
this.audioVideo.transcriptionController?.unsubscribeFromTranscriptEvent(this.transcriptEventHandler););
```