# Creating a service\-linked role for Amazon Chime SDK meetings<a name="create-pipeline-role"></a>

The information in the following sections explains how to create a service\-linked role for Amazon Chime SDK meetings\.

**Topics**
+ [Setting role permissions](#pipeline-role-permissions)
+ [Creating the service\-linked role](#create-sl-role)
+ [Editing the service\-linked role](#edit-pipeline-role)
+ [Deleting the service\-linked role](#delete-pipeline-role)
+ [Regions that support service\-linked roles](#role-supported-regions)

## Setting role permissions<a name="pipeline-role-permissions"></a>

media pipelines use a service\-linked role named *AWSServiceRoleForAmazonChimeSDKMediaPipelines*\. The role allows the capture pipelines to access Amazon Chime SDK meetings on your behalf\. The role trusts the `mediapipelines.chime.amazonaws.com` service\.

The role permissions policy allows the Amazon Chime SDK to complete the following actions on all AWS resources:
+ `chime:CreateAttendee`
+  `chime:DeleteAttendee`
+ `chime:GetMeeting`

You must configure permissions to allow an IAM entity, such as a user, group, or role, to create, edit, or delete a service\-linked role\. For more information about permissions, see [Service linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\. 

## Creating the service\-linked role<a name="create-sl-role"></a>

You use the IAM console to create a service\-linked role for use with Amazon Chime SDK media pipelines\. You must have IAM administrative permissions to complete these steps\. If you don't, contact a system administrator\.

**To create the role**

1. Sign in to the AWS Management Console, and then open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam)

1. In the navigation pane of the IAM console, choose **Roles**, and then choose **Create role**\.

1. Choose the **AWS Service** role type, and then choose **Chime SDK Media Pipelines**\.

   The IAM policy appears\.

1. Select the check box next to the policy, then choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. Edit the description as needed, then choose **Create role**\.

You can also use the AWS CLI or the AWS API to create a service\-linked role named *mediapipelines\.chime\.amazonaws\.com*\. In the AWS CLI, run this command:

```
aws iam create-service-linked-role --aws-service-name mediapipelines.chime.amazonaws.com
```

For more information creating the role, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this role, you can use this same process to create it again\.

## Editing the service\-linked role<a name="edit-pipeline-role"></a>

You can't to edit the *AWSServiceRoleForAmazonChimeSDKMediaPipelines* service\-linked role\. After you create the role, you can't change its name because other entities may reference the role\. However, you can use IAM to edit the role's description\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting the service\-linked role<a name="delete-pipeline-role"></a>

If don't need a service\-linked role, we recommend that you delete it\. To do that, you first delete the media pipelines that use the role\. You can use the AWS CLI or the [DeleteMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_DeleteMediaCapturePipeline.html) API to delete the pipelines\. 

**Using the CLI to delete pipelines**  
Use this command in the AWS CLI to delete media pipelines in your account\.

```
aws chime-sdk-media-pipelines delete-media-capture-pipeline --media-pipeline-id Pipeline_Id
```

**Using an API to delete pipelines**  
Use the [DeleteMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_DeleteMediaCapturePipeline.html) API to delete media pipelines in your account\.

**Deleting the role**  
Once you delete the pipelines, you can use the IAM console, the AWS CLI, or the AWS API to delete the role\. For more information about deleting roles, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Regions that support service\-linked roles<a name="role-supported-regions"></a>

Amazon Chime SDK supports using service\-linked roles in all of the AWS Regions where the service is available\. For more information, see [Amazon Chime SDK endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/chime-sdk.html) in the *Amazon Web Services General Reference*\.