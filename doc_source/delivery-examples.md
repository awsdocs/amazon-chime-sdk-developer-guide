# Delivery examples<a name="delivery-examples"></a>

The following examples show how to process a received `TranscriptEvent`\.

**Note**  
The exact output depends on several factors, including how quickly individuals talk and when they pause\.

## Example 1: StartMeetingTranscription<a name="example-1"></a>

This example shows a typical `StartMeetingTranscription` operation\.

```
meeting.StartMeetingTranscription(
    { EngineTranscribeSettings: { Languagecode: ‘en-US’ } } );
```

The operation generates a `TranscriptEvent`\.

```
{   
    status: {        
        type: 'started',        
        eventTimeMs: 1620118800000,        
        transcriptionConfig: {                    
            LanguageCode: 'en-US'        
        }    
    }
}
```

## Example 2: A partial transcript result<a name="example-2"></a>

In this example, an attendee says, "The quick brown fox jumps over the lazy dog\." Note that in this example, the `isPartial` value is `true`\. If you look deeper into the message, you can see that the system processed the word "fox" as "facts\." The system uses the same `resultId` to update the transcript\. 

```
{
    transcript: {
        results: [{
            resultId:"1",                               isPartial: true,
            startTimeMs: 1620118800000,                 endTimeMs: 1620118801000,
            alternatives: [{
                items:[{
                    type:        'pronunciation',
                    startTimeMs: 1620118800000,         endTimeMs: 1620118800200,
                    attendee: { attendeeId: "1",        externalUserId: "A"},
                    content: "the",                     vocabularyFilterMatch: false
                },
                {
                    type:        'pronunciation',
                    startTimeMs: 1620118800200,          endTimeMs: 1620118800400,
                    attendee: { attendeeId: "1",         externalUserId: "A" },
                    content:"quick",                     vocabularyFilterMatch: false
                },
                {
                    type:'pronunciation',
                    startTimeMs: 1620118800400,          endTimeMs: 1620118800750,
                    attendee: { attendeeId: "1",         externalUserId: "A" },
                    content:"brown",                     vocabularyFilterMatch: false
                },
                {
                    type:'pronunciation',
                    startTimeMs: 1620118800750,          endTimeMs: 1620118801000,
                    attendee:{ attendeeId: "1",          externalUserId: "A" },
                    content:"facts",                     vocabularyFilterMatch: false
                },
                {
                    type:'punctuation',
                    startTimeMs: 1620118801000,          endTimeMs: 1620118801500,
                    attendee:{ attendeeId: "1",          externalUserId: "A" },
                    content:    ",",                     vocabularyFilterMatch: false
                }]
            }]
        }]
    }
}
```

## Example 3: A final transcript result<a name="example-3"></a>

In the event of a partial transcript, the system processes the phrase again\. This example has an `isPartial` value of `false`, and the message contains "fox" instead of "facts\." The system re\-issues the message using the same ID\.

```
{
    transcript: {
        results: [{
            resultId:"1",                                isPartial: false,
            startTimeMs: 1620118800000,                  endTimeMs: 1620118801000,
            alternatives: [{
                items:[{
                    type:        'pronunciation',
                    startTimeMs: 1620118800000,          endTimeMs: 1620118800200,
                    attendee: { attendeeId: "1",         externalUserId: "A"},
                    content: "the",                      vocabularyFilterMatch: false
                },
                {
                    type:        'pronunciation',
                    startTimeMs: 1620118800200,          endTimeMs: 1620118800400,
                    attendee: { attendeeId: "1",         externalUserId: "A" },
                    content:"quick",                     vocabularyFilterMatch: false
                },
                {
                    type:'pronunciation',
                    startTimeMs: 1620118800400,          endTimeMs: 1620118800750,
                    attendee: { attendeeId: "1",         externalUserId: "A" },
                    content:"brown",                     vocabularyFilterMatch: false
                },
                {
                    type:'pronunciation',
                    startTimeMs: 1620118800750,          endTimeMs: 1620118801000,
                    attendee:{ attendeeId: "1",          externalUserId: "A" },
                    content:"fox",                       vocabularyFilterMatch: false
                },
                {
                    type:'punctuation',
                    startTimeMs: 1620118801000,          endTimeMs: 1620118801500,
                    attendee:{ attendeeId: "1",          externalUserId: "A" },
                    content:    ",",                     vocabularyFilterMatch: false
                }]
            }]
        }]
    }
}
```