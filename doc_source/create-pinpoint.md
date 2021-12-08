# Create an Amazon Pinpoint application<a name="create-pinpoint"></a>

To send push notifications, the Amazon Chime SDK requires an Amazon Pinpoint application configured to send pushes to your mobile app\. The following steps explain how to use the AWS console to create a Pinpoint application\.

**To create a Amazon Pinpoint application**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

   If this is your first time using Amazon Pinpoint, you see a page that introduces you to the features of the service\.

1. In the **Get started** section, enter a name for your project, and then choose **Create a project**\.

1. On the **Configure features** page, next to **Push Notifications** choose **Configure**\.

1. On the **Set up push notifications** page, toggle **Apple Push Notification service \(APNs\)**, **Firebase Cloud Messaging \(FCM\)**, or both, and complete the required fields\.
**Important**  
The Amazon Chime SDK currently only supports sending push notifications to APNs and FCM\.

1. When finished, choose **Save\.**

1. Return to the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/) and note the **Project ID** value\. You use that as the ARN for your Amazon Pinpoint application\.