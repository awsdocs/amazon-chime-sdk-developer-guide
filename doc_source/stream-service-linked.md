# Using service\-linked roles for data streaming<a name="stream-service-linked"></a>

The following sections explain how to manage the service\-linked role for data streaming\.

**Topics**
+ [Service\-linked role permissions](#role-permissions)
+ [Creating a service\-linked role](#create-service-linked-role)
+ [Editing a service\-linked role](#editing-roles)
+ [Deleting the resources used by a service\-linked role](#cleaning-up)
+ [Deleting a service\-linked role](#deleting-roles)

## Service\-linked role permissions<a name="role-permissions"></a>

Amazon Chime SDK uses the service\-linked role named **AmazonChimeServiceChatStreamingAccess**\. The role grants access to the AWS services and resources used or managed by Amazon Chime SDK, such as the Kinesis streams used for data streaming\. 

The **AmazonChimeServiceChatStreamingAccess** service\-linked role trusts the following services so that those services can assume the role:
+ chime\.amazonaws\.com

The role permissions policy allows Amazon Chime SDK to complete the following actions on the specified resource:
+ `kms:GenerateDataKey` only when the request is made using `kinesis.*.amazonaws.com`\.
+ `kinesis:PutRecord`, `kinesis:PutRecords`, or `kinesis:DescribeStream` only on streams of the following format: `arn:aws:kinesis:{{region}}:{{account-id}}:stream/chime-messaging-*`\.

You must configure permissions to allow an IAM entity such as a user, group, or role to create, edit, or delete a service\-linked role\. For more information, see [ Service\-linked role permissions ](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM user Guide*\.

## Creating a service\-linked role<a name="create-service-linked-role"></a>

You don't need to manually create a service\-linked role\. When you use the [ PutAppInstanceStreamingConfigurations](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_PutAppInstanceStreamingConfigurations.html) API to create a data streaming configuration, Amazon Chime SDK creates the service\-linked role for you\. 

You can also use the IAM console to create a service\-linked role with the Amazon Chime SDK use case\. In the AWS CLI or the AWS API, create a service\-linked role with the chime\.amazonaws\.com service name\. For more information, see [ Creating a service\-linked role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM user Guide*\. If you delete this role, you can repeat this process to create it again\.

## Editing a service\-linked role<a name="editing-roles"></a>

After you create a service\-linked role, you can only edit its description, and you do that using IAM\. For more information, see [ Editing a service\-linked role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM user Guide*\.

## Deleting the resources used by a service\-linked role<a name="cleaning-up"></a>

Before you can use IAM to delete a service\-linked role, you must first delete any resources used by the role\.

**Note**  
Deletions can fail if you try to delete resources while Amazon Chime SDK is using them\. If a deletion fails, wait a few minutes and try the operation again\.

**To delete resources used by the AmazonChimeServiceChatStreamingAccess role**  
Turn off the data streaming feature for your app instance by invoking the following API\.
+ `aws chime delete-app-instance-streaming-configuration --app-instance-arn {app_instance_arn}`

This action deletes all streaming configurations for your app instance\.

## Deleting a service\-linked role<a name="deleting-roles"></a>

When you no longer need a feature or service that requires a service\-linked role, it's a best practice to delete that role\. Otherwise, you have an unused entity that is not actively monitored or maintained\. However, you must delete the resources used by your service\-linked role before you can manually delete the role\.

You can use the IAM console, AWS CLI, or the AWS API to delete the **AmazonChimeServiceChatStreamingAccess** service\-linked role\. For more information, see [ Deleting a service\-linked role ](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the IAM user Guide\.