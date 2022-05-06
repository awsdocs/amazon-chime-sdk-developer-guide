# Making SDK calls from a backend service<a name="call-from-backend"></a>

Once you create a user to represent your backend services, you create a channel, send messages to that channel, and read messages from that channel\.

Run the following CLI command to create a public channel\.

```
aws chime-sdk-messaging create-channel \
    --chime-bearer "APP_INSTANCE_USER_ARN" \
    --app-instance-arn "APP_INSTANCE_ARN" \
    --name "firstChannel"
```

The command produces an ARN in this format: `arn:aws:chime:region:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID/channel/CHANNEL_ID.`

## How IAM authorization works for backend services<a name="how-iam-works"></a>

In the CLI command from the previous section, note the `chime-bearer` parameter\. It is often useful to know who creates or interacts with resources such as channels and messages, and the `chime-bearer` parameter identifies that user\. Nearly all Amazon Chime SDK messaging APIs take `chime-bearer` as a parameter, except APIs meant to be called only by developers, such as `CreateAppInstance`\.

The IAM permissions for Amazon Chime SDK messaging APIs require an `app-instance-user-arn `that matches the `chime-bearer` parameter\. Additional ARNs—typically channel ARNs—might be required based on the API\. For backend services like the example above, this leads to IAM policies like the following example:

```
{
    "Version": "2012-10-17",
    "Statement": {
    "Effect": "Allow",
    "Action": [
        "chime:SendChannelMessage",
        "chime:ListChannelMessages",
        "chime:CreateChannelMembership",
        "chime:ListChannelMemberships",
        "chime:DeleteChannelMembership",
        "chime:CreateChannel",
        "chime:ListChannels",
        "chime:DeleteChannel",
        ... 
    ],
    "Resource": [
        "arn:aws:chime:region:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID/user/backend_worker",
        "arn:aws:chime:region:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID/channel/*"
    ]
}
```

Note the `AppInstanceUser` ARN and channel ARN in the `Resource` section\. This IAM policy example grants the backend service permission to make API calls as the user with the ID of "backend\-worker\." If you want your backend service to be able to make calls for the people who use your app, change the `APP_INSTANCE_USER_ARN` to `arn:aws:chime:region:AWS_ACCOUNT_ID:app-instance/APP_INSTANCE_ID/user/*`\.

## Understanding implicit API authorization<a name="api-implicit-auth"></a>

In addition to IAM policies, the Amazon Chime SDK messaging APIs have implicit permissions\. For example, an `AppInstanceUser` can only send a message or list a channel membership in channels to which the user belongs\. One exception to this is an `AppInstanceUser` who was promoted to `AppInstanceAdmin`\. By default, admins have permissions to all the channels in your app\. For most use cases, you only need this for backend services that contain significant business logic\.

The following CLI command promotes a backend user to an admin\.

```
aws chime-sdk-identity create-app-instance-admin \
    --app-instance-admin-arn "APP_INSTANCE_USER_ARN" \
    --app-instance-arn "APP_INSTANCE_ARN"
```

## Sending and listing channel messages<a name="send-list-msgs"></a>

The following CLI command sends channel messages\.

```
aws chime-sdk-messaging send-channel-message \
    --chime-bearer "APP_INSTANCE_USER_ARN" \
    --channel-arn "CHANNEL_ARN" \
    --content "hello world" \
    --type STANDARD \
    --persistence PERSISTENT
```

The following CLI commands list channel messages in reverse chronological order\.
+ `aws chime list-channel-messages`
+ `aws chime-sdk-messaging list-channel-messages`

```
aws chime-sdk-messaging list-channel-messages \
    --chime-bearer "APP_INSTANCE_USER_ARN" \
    --channel-arn "CHANNEL_ARN"
```