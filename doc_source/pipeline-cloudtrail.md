# Sending media pipeline events to CloudTrail<a name="pipeline-cloudtrail"></a>

AWS enables CloudTrail for you when you create your AWS account\. When a user calls a supported API in the media pipeline SDK, CloudTrail logs that activity for that API in **Event history**, along with other AWS events\. You can view, search, and download the media pipeline events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) in the *CloudTrail User Guide*\.

For an ongoing record of media pipeline events, you can create a *trail*\. A trail enables CloudTrail to deliver log files to your Amazon S3 bucket\. The following example shows a media pipeline trail\. The data includes the user that called the API, the IAM role used to call the API, and timestamps\. For more information about using CloudTrail see [Logging and monitoring](https://docs.aws.amazon.com/chime-sdk/latest/ag/monitoring-overview.html) in the *Amazon Chime SDK Administrator Guide*\.

```
{
   "Records": [    
   {
      "eventVersion": "1.08",
      "userIdentity": {
          "type": "AssumedRole",
          "principalId": "ABCDEFGHIJKLMNOPQRSTUV:user-name",
          "arn": "arn:aws:sts::123456789101:assumed-role/role-name/user-name",
          "accountId": "109876543210",
          "accessKeyId": "ABCDEFGHIJKLMNOPQRSTUV",
          "sessionContext": {
              "sessionIssuer": {
                  "type": "Role",
                  "principalId": "ABCDEFGHIJKLMNOPQRSTUV",
                  "arn": "arn:aws:iam::109876543210:role/role-name",
                  "accountId": "012345678910",
                  "userName": "user-name"
                  },
          "webIdFederationData": {},
          "attributes": {
              "mfaAuthenticated": "false",
              "creationDate": "2022-03-08T19:34:55Z"
              }
          }
      },
      "eventTime": "2022-03-08T20:28:41Z",
     "eventSource": "chime-sdk-media-pipelines.amazonaws.com",
     "eventName": "CreateMediaCapturePipeline",
     "awsRegion": "us-east-1",
     "sourceIPAddress": "127.0.0.1",
     "userAgent": "[]/[]",
     "requestParameters": {
         "sourceType": "ChimeSdkMeeting",
         "sourceArn": "Hidden_For_Security_Reasons",
         "sinkType": "S3Bucket",
         "sinkArn": "Hidden_For_Security_Reasons",
         "chimeSdkMeetingConfiguration": {
             "artifactsConfiguration": {
                 "audio": {
                    "muxType": "AudioOnly"
                 },
            "video": {
                "state": "Enabled",
                "muxType": "VideoOnly"
                },
            "content": {
                "state": "Enabled",
                "muxType": "ContentOnly"
                }
            }
        }
      },
     "responseElements": {
        "mediaCapturePipeline": {
        "mediaPipelineId": "pipeline-uuid",
        "sourceType": "ChimeSdkMeeting",
        "sourceArn": "Hidden_For_Security_Reasons",
        "status": "Initializing",
        "sinkType": "S3Bucket",
        "sinkArn": "Hidden_For_Security_Reasons",
        "createdTimestamp": "2022-03-08T20:28:41.336Z",
        "updatedTimestamp": "2022-03-08T20:28:41.463Z",
        "chimeSdkMeetingConfiguration": {
            "artifactsConfiguration": {
                "audio": {
                    "muxType": "AudioOnly"
                },
            "video": {
                "state": "Enabled",
                 "muxType": "VideoOnly"
                 },
             "content": {
                 "state": "Enabled",
                 "muxType": "ContentOnly"
                 }
              }
            }
          }
      },
      "requestID": "request-id",
      "eventID": "event-id",
     "readOnly": false,
      "eventType": "AwsApiCall",
      "managementEvent": true,
      "eventCategory": "Management",
      "recipientAccountId": "112233445566",
      "tlsDetails": {
          "tlsVersion": "TLSv1.2",
          "clientProvidedHostHeader": "example.com"
       }
    },  
  ]
}
```