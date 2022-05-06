# Create a service role<a name="create-service-role"></a>

AWS uses service roles to grant permissions to an AWS service so it can access AWS resources\. The policies that you attach to a service role determine which resources the service can access and what it can do with those resources\. The service role that you create for the Amazon Chime SDK gives the service permission to make `SendMessages` calls to your Amazon Pinpoint application\.

**To create a service role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create Policy**\.

1. Choose the **JSON** tab and copy the policy below into the text box\. Be sure to replace `PROJECT_ID` with the the ID of the Amazon Pinpoint application created in the previous step, and the `AWS_ACCOUNT_ID` with your AWS Account ID\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": {
           "Action": "mobiletargeting:SendMessages",
           "Resource": "arn:aws:mobiletargeting:region:AWS_ACCOUNT_ID:apps/PROJECT_ID/messages",
           "Effect": "Allow"
       }
   }
   ```

1. Choose **Next: Tags**\.

1. Choose **Next: Review**, and enter **AmazonChimePushNotificationPolicy** in the **Name** field, and choose **Create Policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, open the **Choose a user case** list and choose **EC2**\.

1. Choose **Next: Permissions**, and in the search box, enter **AmazonChimePushNotificationPolicy**, and select the check box next to the policy\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review**, and enter **ServiceRoleForAmazonChimePushNotification** in the **Name** field\. 
**Important**  
You must use the name listed above\. The Amazon Chime SDK only accepts that specific name\.

1. Choose **Create role**, and on the **Roles** page\. enter **ServiceRoleForAmazonChimePushNotification** in the search box, and choose the matching role\.

1. Choose the **Trust Relationships** tab, choose **Edit trust relationship** and replace the existing policy with the one below\.

   ```
   {
       "Version": "2008-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": {
                   "Service": "messaging.chime.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
            }
       ]
   }
   ```

1. Choose **Update Trust Policy**\.

**Important**  
Modifying the role by changing the name, the permission policy, or the trust policy can break the push notification functionality\. 