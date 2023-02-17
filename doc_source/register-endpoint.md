# Register a mobile device endpoint as an App Instance user<a name="register-endpoint"></a>

To receive push notifications, app instance users must first use the [RegisterAppInstanceUserEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_identity-chime_RegisterAppInstanceUserEndpoint.html) API to register a mobile device\. They must register from a mobile app that has access to the device token for the device's operating system\.

To ensure the app instance User has access to the Amazon Pinpoint application listed in the ARN, the user must have permission to call `mobiletargeting:GetApp` on the Amazon Pinpoint ARN\. Otherwise, the Amazon Chime SDK throws a 403 Forbidden error when calling [RegisterAppInstanceUserEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_identity-chime_RegisterAppInstanceUserEndpoint.html)\.

This example shows the policy needed to register an endpoint\.

```
{ 
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PermissionToRegisterEndpoint",
            "Effect": "Allow",
            "Action": "chime:RegisterAppInstanceUserEndpoint",
            "Resource": "arn:aws:chime:region:aws_account_id:app-instance/app_instance_id/user/app_instance_user_id"
        },
        {
            "Sid": "PermissionToGetAppOnPinpoint",
            "Effect": "Allow",
            "Action": "mobiletargeting:GetApp",
            "Resource": "arn:aws:mobiletargeting:region:aws_account_id:apps/project_id"
        }
    ]
}
```

**To register an endpoint**
+ Use the Amazon Pinpoint ARN and your device token to call the [RegisterAppInstanceUserEndpoint](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_identity-chime_RegisterAppInstanceUserEndpoint.html) API\.