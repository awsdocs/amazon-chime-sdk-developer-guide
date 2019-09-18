# Amazon Chime Events Sent to Chat Bots<a name="events-bots"></a>

The following events are sent to your chat bot from Amazon Chime:
+ **Invite** – Sent when your chat bot is added to an Amazon Chime chat room
+ **Mention** – Sent when a user in a chat room @mentions your chat bot
+ **Remove** – Sent when your chat bot is removed from an Amazon Chime chat room

The following examples show the JSON payload sent to your chat bot for each of these events\.

**Example Invite Event**  

```
            {
              "Sender": {
                            "SenderId": "user@example.com",
                            "SenderIdType": "EmailId"
                         },
              "Discussion": {
                            "DiscussionId": "abcdef12-g34h-56i7-j8kl-mn9opqr012st",
                            "DiscussionType": "Room"
                            },
              "EventType": "Invite",
              "InboundHttpsEndpoint": {
                                        "EndpointType": "Persistent",
                                        "Url": "https://hooks.a.chime.aws/incomingwebhooks/a1b2c34d-5678-90e1-f23g-h45i67j8901k?token=ABCDefGHiJK1LMnoP2Q3RST4uvwxYZAbC56DeFghIJkLM7N8OP9QRsTuV0WXYZABcdefgHiJ"
                                      },
              "EventTimestamp": "2019-04-04T21:27:52.736Z"
            }
```

**Example Mention Event**  

```
            {
                "Sender": {
                            "SenderId": "user@example.com",
                            "SenderIdType": "EmailId"
                          },
                "Discussion": {
                                "DiscussionId": "abcdef12-g34h-56i7-j8kl-mn9opqr012st",
                                "DiscussionType": "Room"
                              },
                "EventType": "Mention",
                "InboundHttpsEndpoint": {
                                            "EndpointType": "ShortLived",
                                            "Url": "https://hooks.a.chime.aws/incomingwebhooks/a1b2c34d-5678-90e1-f23g-h45i67j8901k?token=ABCDefGHiJK1LMnoP2Q3RST4uvwxYZAbC56DeFghIJkLM7N8OP9QRsTuV0WXYZABcdefgHiJ"
                                        },
                "EventTimestamp": "2019-04-04T21:30:43.181Z",
                "Message": "@botDisplayName@example.com Hello Chatbot"
            }
```

**Note**  
The `InboundHttpsEndpoint` URL for a Mention event expires 2 minutes after it is sent\.

**Example Remove Event**  

```
            {
                "Sender": {
                            "SenderId": "user@example.com",
                            "SenderIdType": "EmailId"
                          },
                "Discussion": {
                                "DiscussionId": "abcdef12-g34h-56i7-j8kl-mn9opqr012st",
                                "DiscussionType": "Room"
                              },
                "EventType": "Remove",
                "EventTimestamp": "2019-04-04T21:27:29.626Z"
            }
```