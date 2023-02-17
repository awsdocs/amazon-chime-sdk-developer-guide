# Granting developers permissions to the Amazon WorkDocs API<a name="wd-iam-sameacct"></a>

**Note**  
For greater security, create federated users instead of IAM users whenever possible\.

If you are an IAM administrator, you can grant Amazon WorkDocs API access to an IAM user from the same AWS account\. To do this, create a Amazon WorkDocs API permission policy and attach it to the IAM user\. The following API policy grants read\-only permission to the various `Describe` APIs\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "WorkDocsAPIReadOnly",
            "Effect": "Allow",
            "Action": [
                "workdocs:Get*",
		    "workdocs:Describe*"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```