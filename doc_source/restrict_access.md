# Restricting access to a specific Amazon WorkDocs instance<a name="restrict_access"></a>

If you have multiple Amazon WorkDocs sites on an AWS account and you want to grant API access to a specific site, you can define a `Condition` element\. The `Condition` element lets you specify conditions for when a policy is in effect\.

The following example shows a condition element:

```
    "Condition": 
    {
                "StringEquals": {
                    "Resource.OrganizationId": "d-123456789c5"
                }
    }
```

With the above condition in place in a policy, users can only access the Amazon WorkDocs instance with the ID of `d-123456789c5`\. Amazon WorkDocs Instance ID is sometimes referred as Organization ID or Directory ID\. For more information, see [Restricting access to a specific Amazon WorkDocs instance](#restrict_access)\. 

Follow these steps to obtain a Amazon WorkDocs organization ID from the AWS console:

**To get an organization ID**

1. In the [AWS Directory Service console](https://console.aws.amazon.com/directoryservicev2/) navigation pane, choose **Directories**\.

1. Note the **Directory ID** value that corresponds to your Amazon WorkDocs site\. That is the Organization ID for the site\.