# Setting up notifications for an IAM user or role<a name="manage-notifications"></a>

To create and manage notifications in Amazon WorkDocs, administrators use the IAM and Amazon WorkDocs consoles\. You use the IAM console to set user permissions, and you use the Amazon WorkDocs console to enable notifications\. Once you enable notifications, you then subscribe to them\. Follow these steps\.

**Note**  
For greater security, create federated users instead of IAM users whenever possible\.

**To set IAM user permissions**
+ Use the IAM console to set the following permissions for the user:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
          "Effect": "Allow",
          "Action": [
              "workdocs:CreateNotificationSubscription",
              "workdocs:DeleteNotificationSubscription",
              "workdocs:DescribeNotificationSubscriptions"
              ],
          "Resource": "*"
          }
      ]
  }
  ```

**To enable notifications**

Enabling notifications allows you to call [CreateNotificationSubscription](https://docs.aws.amazon.com/workdocs/latest/APIReference/API_CreateNotificationSubscription.html) after you subscribe to notifications\.

1. Open the Amazon WorkDocs console at [https://console\.aws\.amazon\.com/zocalo/](https://console.aws.amazon.com/zocalo/)\.

1. On the **Manage Your WorkDocs Sites** page, select the desired directory and choose **Actions** and then **Manage Notifications**\.

1. On the **Manage Notifications** page, choose **Enable Notifications**\.

1. Enter the ARN for the user or role you want to allow to receive notifications from your Amazon WorkDocs site\.

For information about enabling Amazon WorkDocs to use notifications, see [ Using the Amazon WorkDocs API with the AWS SDK for Python and AWS Lambda](https://aws.amazon.com/blogs/business-productivity/using-the-amazon-workdocs-api-with-the-aws-sdk-for-python-and-aws-lambda/)\. Once you enable notifications, you and your user can subscribe to them\. 

**To subscribe to WorkDocs notifications**

1. Prepare your endpoint to process Amazon SNS messages\. For more information, see [Fanout to HTTP/S endpoints](https://docs.aws.amazon.com/sns/latest/dg/SendMessageToHttp.html#SendMessageToHttp.prepare) in the *Amazon Simple Notification Service Developer Guide*\. 
**Important**  
SNS sends a confirmation message to your configured endpoint\. You *must* confirm this message in order to receive notifications\. Also, if you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

1. Do the following:
   + Get an organization ID

     1. In the [AWS Directory Service console](https://console.aws.amazon.com/directoryservicev2/) navigation pane, select **Directories**\. 

     1. The **Directory ID** corresponding to your Amazon WorkDocs site also serves as the Organization ID for that site\.
   + Create the subscription request as follows:

     ```
     CreateNotificationSubscriptionRequest request = new CreateNotificationSubscriptionRequest();
     request.setOrganizationId("d-1234567890");
     request.setProtocol(SubscriptionProtocolType.Https);
     request.setEndpoint("https://my-webhook-service.com/webhook");
     request.setSubscriptionType(SubscriptionType.ALL);
     CreateNotificationSubscriptionResult result = amazonWorkDocsClient.createNotificationSubscription(request);
     System.out.println("WorkDocs notifications subscription-id: " result.getSubscription().getSubscriptionId());
     ```



**SNS Notifications**

The message includes the following information:
+ `organizationId` — The ID of the organization\.
+ `parentEntityType` — The type of the parent \(`Document` \| `DocumentVersion` \| `Folder`\)\.
+ `parentEntityId` — The ID of the parent\.
+ `entityType` — The type of the entity \(`Document` \| `DocumentVersion` \| `Folder`\)\.
+ `entityId` — The ID of the entity\.
+ action — The action, which can be one of the following values:
  + `delete_document`
  + `move_document`
  + `recycle_document`
  + `rename_document`
  + `revoke_share_document`
  + `share_document`
  + `upload_document_version`

**To disable notifications**

1. Open the Amazon WorkDocs console at [https://console\.aws\.amazon\.com/zocalo/](https://console.aws.amazon.com/zocalo/)\.

1. On the **Manage Your WorkDocs Sites** page, select the desired directory and choose **Actions** and then **Manage Notifications**\.

1. On the **Manage Notifications** page, select the ARN that you wish to disable notifications for and choose **Disable Notifications**\.