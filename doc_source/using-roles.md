# Using service\-linked roles<a name="using-roles"></a>

Amazon Chime uses AWS Identity and Access Management \(IAM\) [ service\-linked roles ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that links directly to Amazon Chime\. Amazon Chime predefines the service\-linked roles, and they include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Amazon Chime more efficient, because you aren't required to manually add the necessary permissions\. Amazon Chime defines the permissions of its service\-linked roles, and unless defined otherwise, only Amazon Chime can assume its roles\. The defined permissions include the trust and permissions policies\. The permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting its related resources\. This protects your Amazon Chime resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [ AWS services that work with IAM ](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html)\. Look for the services that have **Yes** in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the documentation for that service\.

**Topics**
+ [Using service\-linked roles for data streaming](stream-service-linked.md)