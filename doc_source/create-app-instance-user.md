# Creating an AppInstanceUser<a name="create-app-instance-user"></a>

Once you create an `AppInstance`, you create an `AppInstanceUser` in that `AppInstance`\. You typically do this when a user first registers or logs in to your app\. You can also create an `AppInstanceUser` that acts on behalf of your backend services\.

The following example shows how to create a backend `AppInstanceUser`:

```
aws chime create-app-instance-user \
    --app-instance-arn "APP_INSTANCE_ARN" \
    --app-instance-user-id "backend-worker" \
    --name "backend-worker"
```

In the create response, note the `AppInstanceUserArn`\. It takes this form: `arn:aws:chime:us-east-1:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID/user/APP_INSTANCE_USER_ID`\. In this example, `APP_INSTANCE_USER_ID` is "backend\-worker\."

**Note**  
As a best practice, when creating an `AppInstanceUser` for a client application, have the `AppInstanceId` match an existing unique ID for that user, such as the `sub` of an identity provider\. The name is an optional placeholder that is attached to some API entities, such as a message sender\. It allows you to control the display name of a user in one place, rather then needing to look it up from `AppInstanceUser` ARN, which is also attached as the sender of a message\.