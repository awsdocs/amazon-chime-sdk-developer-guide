# Authenticating end\-user client applications<a name="auth-client-apps"></a>

You can also run Amazon Chime SDK messaging from end\-user client applications\. [Making SDK calls from a backend service](call-from-backend.md) explains how to make API calls such as create\-channel, send\-channel\-message, and list\-channel\-messages\. End user client applications such as browsers and mobile applications make these same API calls\. Client applications can also connect via WebSocket to receive real time updates on messages and events to channels they are members of\. This section covers how to give IAM credentials to a client application scoped to a specific app instance user\. Once the end users have these credentials, they can make the API calls shown in [Making SDK calls from a backend service](call-from-backend.md)\. To see a full demo of a client application, see [ https://github\.com/aws\-samples/amazon\-chime\-sdk/tree/main/apps/chat](https://github.com/aws-samples/amazon-chime-sdk/tree/main/apps/chat)\. For more information about receiving realtime messages from the channels that a client app belongs to, see [Using WebSockets to receive messages](websockets.md)\.

## Providing IAM credentials to end users<a name="connect-id-provider"></a>

Amazon Chime SDK messaging integrates natively with AWS Identity and Access Management \(IAM\) policies to authenticate incoming requests\. The IAM policy defines what an individual user can do\. IAM policies can be crafted to provide scoped\-down limited credentials for your use case\. For more information on creating policies for Amazon Chime SDK messaging users, see [Example IAM roles](iam-roles.md)\.

If you have an existing identity provider, you have the following options for integrating your existing identity with Amazon Chime SDK messaging\.
+ You can use your existing identity provider to authenticate users and then integrate the authentication service with AWS Security Token Service \(STS\) to create your own credential vending service for clients\. STS provides APIs for assuming IAM Roles\.
+ If you already have a SAML or OpenID compatible identity provider, Amazon Chime recommends using Amazon [Cognito Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html), which abstract away calls to AWS STS [AssumeRoleWithSAML](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html) and [AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html)\. Amazon Cognito integrates with OpenID, SAML, and public identity providers such as Facebook, Login with Amazon, Google, and Sign in with Apple\.

If you do not have an identity provider, you can get started with Amazon Cognito User Pools\. For an example of how to use Amazon Cognito with the Amazon Chime SDK messaging features, see [ Build chat features into your application with Amazon Chime SDK messaging](http://aws.amazon.com/blogs/business-productivity/build-chat-features-into-your-application-with-amazon-chime-sdk-messaging/)\. 

Alternately, you can use the [AWS STS](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html) to create your own credential vending service or build your own identity provider\.

**Using STS to vend credentials**  
If you already have an IDP such as ActiveDirectory LDAP, and you want to implement a custom credential vending service, or grant access to chat for non\-authenticated meeting attendees, you can use the [AWS STS AssumeRole API](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html)\. To do this, you first create a Chime Messaging SDK Role\. For more information about creating that role, see [ Creating a role to delegate permissions to an IAM user ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)\.

The IAM Role would have permissions to the Chime Messaging SDK action your application would use, similar to the following:

```
{
    "Version": "2012-10-17",
    "Statement": [
         {
            "Effect": "Allow",
            "Action": [
                "chime:GetMessagingSessionEndpoint"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "chime:SendChannelMessage",
                "chime:ListChannelMessages",
                "chime:CreateChannelMembership",
                "chime:ListChannelMemberships",
                "chime:DeleteChannelMembership",
                "chime:CreateChannelModerator",
                "chime:ListChannelModerators",
                "chime:DescribeChannelModerator",
                "chime:CreateChannel",
                "chime:DescribeChannel",
                "chime:ListChannels",
                "chime:DeleteChannel",
                "chime:RedactChannelMessage",
                "chime:UpdateChannelMessage",
                "chime:Connect",
                "chime:ListChannelBans",
                "chime:CreateChannelBan",
                "chime:DeleteChannelBan",
                "chime:ListChannelMembershipsForAppInstanceUser"
                "chime:AssociateChannelFlow",
                "chime:DisassociateChannelFlow",
                "chime:GetChannelMessageStatus"
            ],
            "Resource": [
                "{Chime_App_Instance_Arn}/user/${my_applications_user_id}",
                "{Chime_App_Instance_Arn}/channel/*"
            ]
        }
    ]
}
```

For this example, call this role the *ChimeMessagingSampleAppUserRole*\.

Note the session tag in the *ChimeMessagingSampleAppUserRole* policy `${my_application_user_id}` in the user ARN resource\. This session tag is parameterized in the [ AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API call to limit the credentials returned to permissions for a single user\.

The [ AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API call is called using an already credentialed IAM entity such as an IAM user\. It can also be called by a different IAM role such as an [AWS Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)\. That IAM identity must have permissions to call `AssumeRole` on the *ChimeMessagingSampleAppUserRole*\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
         {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::MY_AWS_ACCOUNT_ID:role/ChimeMessagingSampleAppUserRole"
        }
    ]
}
```

 For this example, call this role the *ChimeSampleAppServerRole*\.

You need to set up the *ChimeMessagingSampleAppUserRole* with a trust policy that allows the *ChimeMessagingSampleAppServerRole* to call the [STS AssumeRole API](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) on it\. For more information about using trust policies with IAM roles, see [ How to use trust policies with IAM roles ](http://aws.amazon.com/blogs/security/how-to-use-trust-policies-with-iam-roles/)\. You can use the AWS IAM Roles Console to add this policy to the *ChimeMessagingSampleAppUserRole*\. The following example shows a typical trust relationship\.

```
{
    "Version": "2012-10-17",
    "Statement": [
         {
            "Effect": "Allow",
            "Principal": {
               "AWS":"arn:aws:iam::MY_AWS_ACCOUNT_ID:role/ChimeMessagingSampleAppServerRole"
            }
            "Action": "sts:AssumeRole"
        }
    ]
}
```

 In a sample deployment, an [Amazon EC2](https://aws.amazon.com/ec2/) instance, or an AWS Lambda is launched with the `ChimeMessagingSampleAppServerRole`\. The server then:

1. Performs any application specific authorization on a client's requests to receive credentials\.

1. Calls STS `AssumeRole` on `ChimeMessagingSampleAppUserRole`, with a tag parameterizing the `${aws:PrincipalTag/my_applications_user_id}`\.

1. Forwards the credentials returned in the `AssumeRole` call to the user\.

The following example shows CLI command for assuming a role for step 2:

`aws sts assume-role --role-arn arn:aws:iam::MY_AWS_ACCOUNT_ID:role/ChimeMessagingSampleAppUserRole --role-session-name demo --tags Key=my_applications_user_id,Value=123456789 ` 