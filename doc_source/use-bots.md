# Use Chat Bots with Amazon Chime<a name="use-bots"></a>

If you administer an Amazon Chime Enterprise account, you can create up to 10 chat bots for integration with Amazon Chime\. Chat bots can only be used in chat rooms created by members of your account\. Only chat room administrators can add chat bots to a chat room\. After a chat bot is added to a chat room, members of the chat room can interact with the bot using commands provided by the bot creator\. For more information, see [Using Chat Bots](https://docs.aws.amazon.com/chime/latest/ug/chat-bots.html) in the *Amazon Chime User Guide*\.

You can also use the Amazon Chime API operation to enable or stop chat bots for your Amazon Chime account\. For more information, see [Update Chat Bots](update-bots.md)\.

**Note**  
Chat bots cannot be deleted\. To stop a chat bot from being used in your account, use the Amazon Chime [UpdateBot](https://docs.aws.amazon.com/chime/latest/APIReference/API_UpdateBot.html) API operation in the *Amazon Chime API Reference*\. When you stop a chat bot, chat room administrators can remove it from a chat room, but they cannot add it to a chat room\. Users who @mention a stopped chat bot in a chat room receive an error message\.

## Prerequisites<a name="bots-prereqs"></a>

Before you start the procedure to integrate chat bots with Amazon Chime, complete the following prerequisites:
+ Create a chat bot\.
+ Create the outbound endpoint for Amazon Chime to send events to your bot\. Choose from an AWS Lambda function ARN or an HTTPS endpoint\. For more information about Lambda, see the *[AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)*\.

## DNS Best Practices for HTTPS Endpoints<a name="dns-practices"></a>

We recommend the following best practices when assigning DNS for your HTTPS endpoint:
+ Use a DNS subdomain that is dedicated to the bot endpoint\.
+ Use only A\-records to point to the bot endpoint\.
+ Protect your DNS servers and DNS registrar account to prevent domain hijacking\.
+ Use publicly valid TLS intermediate certificates that are dedicated to the bot endpoint\.
+ Cryptographically verify the bot message signature before acting on a bot message\.

After creating your chat bot, use the AWS Command Line Interface \(AWS CLI\) or the Amazon Chime API operation to complete the tasks described in the following sections\.

**Topics**
+ [Step 1: Integrate a Chat Bot with Amazon Chime](integrate-bots.md)
+ [Step 2: Configure the Outbound Endpoint for an Amazon Chime Chat Bot](config-endpoints.md)
+ [Step 3: Add the Chat Bot to an Amazon Chime Chat Room](add-bots.md)
+ [Authenticate Chat Bot Requests](auth-bots.md)
+ [Update Chat Bots](update-bots.md)