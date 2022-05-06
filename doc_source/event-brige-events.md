# Creating failure alerts by automating with EventBridge<a name="event-brige-events"></a>

The Amazon Chime SDK delivers Events when there is an error invoking your processor Lambda function\. Events are sent regardless of the fallback action specified for the processor when creating a channel flow\. You can write simple rules to specify these events, plus the automated actions to take when any of those events matches a rule\. For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\. When errors like these occur, then depending on the fallback action that you configure, members on the channel can't send messages, or messages will flow through the channel with no processing\. For more information about the Fallback action, refer to [Processor](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_Processor.html) in the Amazon Chime SDK API reference\. 

This example shows shows a typical failure event\.

```
{
    "version": "0",
    "id": "12345678-1234-1234-1234-111122223333",
    "detail-type": "Chime ChannelFlow Processing Status",
    "source": "aws.chime",
    "account": "111122223333",
    "time": "yyyy-mm-ddThh:mm:ssZ",
    "region": "region",
    "resources": [],
    "detail": {
        "eventType": "ProcessorInvocationFailure",
        "appInstanceArn": "arn:aws:chime:region:AWSAccountId:app-instance/AppInstanceId",
        "channelArn": "arn:aws:chime:region:AWSAccountId:app-instance/AppInstanceId/channel/ChannelId",
        "messageId": "298efac7298efac7298efac7298efac7298efac7298efac7298efac7298efac7",
        "processorResourceArn": "arn:aws:lambda:region:AWSAccountId:function:ChannelFlowLambda",
        "failureReason": "User is not authorized to perform: lambda:InvokeFunction on resource: arn:aws:lambda:region:AppInstanceId:function:ChannelFlowLambda because no resource-based policy allows the lambda:InvokeFunction action"
      }
}
```