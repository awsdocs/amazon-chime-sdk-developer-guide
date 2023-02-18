# Defining an IAM policy<a name="define-iam-policy"></a>

To start, define an IAM policy that gives you permission to establish a WebSocket connection\. The following example policy gives an `AppInstanceUser` permission to establish a WebSocket connection\.

```
"Version": "2012-10-17",
"Statement": [
  {
    "Effect": "Allow",
    "Action: [
      "chime:Connect"
    ],
    "Resource": [
      "arn:aws:chime:region:{aws_account_id}:app-instance/{app_instance_id}/user/{app_instance_user_id}"
    ]
 },
 {
    "Effect": "Allow",
    "Action: [
      "chime:GetMessagingSessionEndpoint"
    ],
    "Resource": [
      "*"
    ]
 }
 ]
}
```