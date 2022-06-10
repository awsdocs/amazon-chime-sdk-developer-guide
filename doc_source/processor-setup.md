# Setting up a Channel Processor<a name="processor-setup"></a>

To start using channel flows, you first create a processor Lambda function to handle preprocessing for your use case\. For example, you can update message content or metadata, deny messages and prevent them from being sent, or let the original message through\.

**Prerequisites**
+ The Lambda function must be in the same AWS account and the same AWS Regions as the AppInstance\.

**Granting invocation permissions**  
You must give the Amazon Chime SDK messaging service permssion to invoke your Lambda resource\. For more information about permissions, refer to [Using resource\-based policies for AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)\. For example:

  
**Principal**: chime\.amazonaws\.com  
**Action**: lambda:InvokeFunction  
**Effect**: Allow  
**AWS:SourceAccount**:*Your AWS account ID*\.  
**AWS:SourceArn**: `"arn:aws:chime:region:AWS account ID:appInstance/*"`

**Note**  
You can provide a specific app instance ID to invoke your processor, or use a wildcard to allow all Amazon Chime SDK app instances in an account to invoke your processor\. 

**Granting callback permissions**  
You also need to allow your processor Lambda functions to call the `ChannelFlowCallback` API\. For information about doing that, see [AWS Lambda run role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) in the *AWS Lambda developer guide*\. 

You can add an Inline policy to your Lambda function's run role\. This example allows the processor to invoke the `ChannelFlowCallback API`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "chime:ChannelFlowCallback"
            ],
            "Resource arn:aws:chime:region:AWS account ID:appInstance/*",
            "Effect": "Allow"
        }
    ]
}
```

**Note**  
Follow the best practices for Lambda functions\. For more information, refer to these topics:   
[ Performance Efficiency Best Practices](https://docs.aws.amazon.com/whitepapers/latest/serverless-architectures-lambda/performance-efficiency-best-practices.html) 
[Best practices for working with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
[Configuring reserved concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html#configuration-concurrency-reserved)
[Asynchronous invocation](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html)

**Invoking processor Lambda functions**  
When a user sends a message, the following input request invokes the processor Lambda function\.

```
{
    "EventType": "string"
    "CallbackId": "string"
    "ChannelMessage": {
        "MessageId": "string",
        "ChannelArn": "string",
        "Content": "string",
        "Metadata": "string",
        "Sender":{
            "Arn": "string", 
            "Name": "string"
        },
        "Persistence": "string",
        "LastEditedTimestamp": "string", 
        "Type": "string",
        "CreatedTimestamp": "string", 
    }
}
```

EventType  
The event being sent to processor\. The value is a `CHANNEL_MESSAGE_EVENT` constant\.

CallbackId  
The token used while calling the `ChannelFlowCallback` API from the processor\.

ChannelMessage  
*ChannelArn*   The ARN of the channel  
*Content*   Message content to be processed  
*CreatedTimestamp*   The time at which the message was created  
*LastEditedTimestamp*   The time at which a message was edited  
*MessageId*   The message identifier  
*Metadata*   Message metadata to be processed  
*Persistence*   Boolean that controls whether the message is persisted on the back end\. Valid Values: `PERSISTENT | NON_PERSISTENT`  
*Sender*   The message sender\. Type: an [identity object](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_Identity.html)\.  
*Type*   The message type\. ChannelFlow only supports the `STANDARD` message types\. Valid Value: `STANDARD`

The processor function determines the following about each message\.
+ Whether to update the message content, metadata, or both
+ Whether to deny a message 
+ Whether to leave a message unchanged

When processing finishes, the processor Lambda function sends the result back to the Amazon Chime SDK Messaging service so the message can be sent to all recipients\. Message status is marked `PENDING` until the processor Lambda function sends back the results\. The processor Lambda function has 48 hours to send back the results\. We do not guarantee message delivery after that, and the `ChannelFlowCallback` API throws a Forbidden Exception error message\. To send back the results, invoke the [ChannelFlowCallback](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_ChannelFlowCallback.html) API\.