# Parsing transcripts<a name="parse-transcripts"></a>

Use the following command to parse transcription content from a transcription message\. The command parses complete sentences from the transcript\-message\.txt files\.

```
with open('transcript-message.txt') as f:
        for line in f:
            result_json = json.loads(line)["transcript"]["results"][0]
            if result_json['isPartial'] == False:
                print(result_json["alternatives"][0]["transcript"])
```