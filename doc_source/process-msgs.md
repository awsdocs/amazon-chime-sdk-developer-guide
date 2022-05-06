# Transcription messages<a name="process-msgs"></a>

The Amazon Chime SDK service shares transcription information with attendees by sending `TranscriptEvent` objects in data messages\. A `TranscriptEvent` delivers a `Transcript` or a `TranscriptionStatus`\. 

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
    eventTimeMs:                   number;
    transcriptionRegion:           string;
    transcriptionConfiguration:    string;
    message?:                      string;
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
    entities?: TranscriptEntity[];
}

export default class TranscriptItem {
    type:                      TranscriptItemType;
    startTimeMs:               number;
    endTimeMs:                 number;
    attendee:                  Attendee;
    content:                   string;
    vocabularyFilterMatch?:    boolean;
    confidence?:               number;  
    stable?:                   boolean;
}

enum TranscriptItemType {
    PRONUNCIATION    =    'pronunciation',// content is a word
    PUNCTUATION      =    'punctuation',// content is punctuation
}

export default class TranscriptEntity {  
    category:       string;  
    confidence:     number;  
    content:        string;  
    endTimeMs:      number;  
    startTimeMs:    number;  
    type?:          string;
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

1. The `transcription.results[i].alternatives[j].items[k].endTimeMs` field can generally be ignored, but is supplied for completeness of who said what when\.

1. `transcription.results[i].alternatives[j].items[k].vocabularyFilterMatch` is true if the content matched a word in the filter, otherwise it is false\.

1. `transcription.results[i].alternatives[j].items[k].confidence` is a value between 0 and 1\. It indicates the engine's confidence that the item content correctly matches the spoken word, with 0 being the lowest confidence and 1 being the highest confidence\.

1. `transcription.results[i].alternatives[j].items[k].stable` indicates whether the current word will change in future partial result updates\. This value can only be true if you enable the partial results stabilization feature by setting `EnablePartialResultsStabilization` to `true` in your request\.

1. `transcription.results[i].alternatives[j].entities` includes an entry for each entity that the Content Identification or Redaction features detect\. The list is only populated if you enable Content Identification or Redaction\. An entity can be data such as personally indentifiable information or personal health information\. You can use entities to highlight, or take action on, words of interest during transcription\.

1. `transcription.results[i].alternatives[j].entities[k].category` is the entity's category\. It equals the Content Identification or Redaction type, such as "PII" or "PHI", which is provided in the request\.

1. `transcription.results[i].alternatives[j].entities[k].confidence` measures how strong the engine is that the particular content is truly an entity\. Note that this is different than the item\-level confidence, which measures how confident the engine is in the correctness of the words themselves\.

1. `transcription.results[i].alternatives[j].entities[k].content` is the actual text that makes up the entity\. This can be multiple items, such as an address\.

1. `transcription.results[i].alternatives[j].entities[k].startTimeMs` captures the time at which the entity started being spoken\.

1. `transcription.results[i].alternatives[j].entities[k].endTimeMs` captures the time at which the entity finished being spoken\.

1. `transcription.results[i].alternatives[j].entities[k].type` is only supported for the Transcribe engine and provides the sub\-type of the entity\. These are values such as `ADDRESS`, `CREDIT\_DEBIT\_NUMBER`, and so on\.

## Registering event handlers for TranscriptEvents<a name="register-handler"></a>

The following examples use the Amazon Chime SDK for Javascript\. However, the pattern is consistent across all Amazon Chime SDKs\.

The `TranscriptionController` in the `RealtimeController` and `RealtimeControllerFacade` includes specific functions for adding a handler thata processes `TranscriptionEvents`:

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

`DefaultRealtimeController` also takes an optional `TranscriptionController` object in its constructor\. That allows you to override the `DefaultTranscriptionController` behavior\. Developer applications subscribe and unsubscribe to one or more callbacks through the `TranscriptionController` object of the `AudioVideoFacade` object:

```
// Subscribe
this.audioVideo.transcriptionController?.subscribeToTranscriptEvent(this.transcriptEventHandler);

// Unsubscribe
this.audioVideo.transcriptionController?.unsubscribeFromTranscriptEvent(this.transcriptEventHandler););
```