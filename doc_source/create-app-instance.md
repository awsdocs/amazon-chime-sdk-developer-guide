# Creating an AppInstance<a name="create-app-instance"></a>

To use Amazon Chime SDK messaging, you must first create an Amazon Chime SDK AppInstance in your AWS account\.

**To create an `AppInstance` for messaging**

1. In the CLI, run `aws chime-sdk-identity create-app-instance --name NameOfAppInstance.`

1. In the create response, make note of the `AppInstanceArn`\. `arn:aws:chime:region:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID`\.