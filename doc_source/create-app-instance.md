# Creating an app instance<a name="create-app-instance"></a>

To use Amazon Chime SDK messaging, you must first create an Amazon Chime app instance within your AWS account\.

**To create an app instance for messaging**

1. Create an Amazon Chime app instance\. `aws chime create-app-instance --name >NameOfAppInstance<`

1. In the create response, make note of the `AppInstanceArn`\. You use this to create an IAM policy for your users\. 