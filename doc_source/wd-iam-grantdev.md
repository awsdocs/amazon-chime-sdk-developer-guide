# Granting users permission to assume an IAM role<a name="wd-iam-grantdev"></a>

A developer with an administrative AWS account can allow a user to assume an IAM role\. To do that, you create a new policy and attach it to that user\.

The policy must include a statement with the `Allow` effect on the `sts:AssumeRole` action, plus the Amazon Resource Name \(ARN\) of the role in a `Resource` element, as shown in the following example\. Users that get the policy, either through group membership or direct attachment, can switch to the specified role\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "sts:AssumeRole",
    "Resource": "arn:aws:iam::<aws_account_id>:role/workdocs_app_role"
  }
}
```