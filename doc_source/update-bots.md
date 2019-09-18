# Update Chat Bots<a name="update-bots"></a>

As the Amazon Chime account administrator, you can use the Amazon Chime API with the AWS SDK or AWS CLI to view your chat bot details\. You can also enable or stop your chat bots from being used in your account\. You can also regenerate security tokens for your chat bot\. 

For more information, see the following topics in the *Amazon Chime API Reference*:
+ [GetBot](https://docs.aws.amazon.com/chime/latest/APIReference/API_GetBot.html) – Gets your chat bot details, such as bot email address and bot type\.
+ [UpdateBot](https://docs.aws.amazon.com/chime/latest/APIReference/API_UpdateBot.html) – Enables or stops a chat bot from being used in your account\.
+ [RegenerateSecurityToken](https://docs.aws.amazon.com/chime/latest/APIReference/API_RegenerateSecurityToken.html) – Regenerates the security token for your chat bot\.

You can also change the `PutEventsConfiguration` for your chat bot\. For example, if your chat bot was initially configured to use an outbound HTTPS endpoint, you can delete the previous events configuration and put a new events configuration for a Lambda function ARN\.

For more information, see the following topics in the *Amazon Chime API Reference*:
+ [DeleteEventsConfiguration](https://docs.aws.amazon.com/chime/latest/APIReference/API_DeleteEventsConfiguration.html)
+ [PutEventsConfiguration](https://docs.aws.amazon.com/chime/latest/APIReference/API_PutEventsConfiguration.html)