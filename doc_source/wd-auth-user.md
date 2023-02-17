# Authentication and access control for user applications<a name="wd-auth-user"></a>

Amazon WorkDocs user level applications are registered and managed through the Amazon WorkDocs console\. Developers should register their applications on the `My Applications` page on the Amazon WorkDocs console which will provide unique IDs for each application\. During registration, developers should specify redirect URIs where they will receive access tokens as well as application scopes\.

Currently, applications can only access Amazon WorkDocs sites within the same AWS account where they are registered\.

**Topics**
+ [Granting permissions to call the Amazon WorkDocs APIs](#api-auth)
+ [Using folder IDs in API calls](#use-folder-ids)
+ [Create an application](#wd-app-create-app)
+ [Application scopes](#wd-app-scopes)
+ [Authorization](#wd-authorization)
+ [Invoking Amazon WorkDocs APIs](#wd-auth-invoke)

## Granting permissions to call the Amazon WorkDocs APIs<a name="api-auth"></a>

Command line interface users must have full permissions to Amazon WorkDocs and AWS Directory Service\. Without the permissions, any API calls return **UnauthorizedResourceAccessException** messages\. The following policy grants full permissions\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
          "workdocs:*",
          "ds:*",
          "ec2:CreateVpc",
          "ec2:CreateSubnet",
          "ec2:CreateNetworkInterface",
          "ec2:CreateTags",
          "ec2:CreateSecurityGroup",
          "ec2:DescribeVpcs",
          "ec2:DescribeSubnets",
          "ec2:DescribeNetworkInterfaces",
          "ec2:DescribeAvailabilityZones",
          "ec2:AuthorizeSecurityGroupEgress",
          "ec2:AuthorizeSecurityGroupIngress",
          "ec2:DeleteSecurityGroup",
          "ec2:DeleteNetworkInterface",
          "ec2:RevokeSecurityGroupEgress",
          "ec2:RevokeSecurityGroupIngress"
          ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

If you want to grant read\-only permissions, use this policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
          "workdocs:Describe*",
          "ds:DescribeDirectories",       
          "ec2:DescribeVpcs",
          "ec2:DescribeSubnets"
          ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

In the policy, the first action grants access to all the Amazon WorkDocs `Describe` operations\. The `DescribeDirectories ` action obtains information about your AWS Directory Service directories\. The Amazon EC2 operations enable Amazon WorkDocs to obtain a list of your VPCs and subnets\.

## Using folder IDs in API calls<a name="use-folder-ids"></a>

Whenever an API call accesses a folder, you must use the folder ID, not the folder name\. For example, if you pass `client.get_folder(FolderId='MyDocs')`, the API call returns an **UnauthorizedResourceAccessException** message and the following 404 message\.

```
client.get_folder(FolderId='MyDocs')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\user-name\AppData\Local\Programs\Python\Python36-32\lib\site-packages\botocore\client.py", line 253, in _api_call
    return self._make_api_call(operation_name, kwargs)
  File "C:\Users\user-name\AppData\Local\Programs\Python\Python36-32\lib\site-packages\botocore\client.py", line 557, in _make_api_call
    raise error_class(parsed_response, operation_name)
botocore.errorfactory.UnauthorizedResourceAccessException: An error occurred (UnauthorizedResourceAccessException) when calling the GetFolder operation: 
Principal [arn:aws:iam::395162986870:user/Aman] is not allowed to execute [workdocs:GetFolder] on the resource.
```

To avoid that, use the ID in the folder's URL\. 

`site.workdocs/index.html#/folder/abc123def456ghi789jkl789mno4be7024df198736472dd50ca970eb22796082e3d489577`\.

Passing that ID returns a correct result\.

```
client.get_folder(FolderId='abc123def456ghi789jkl789mno4be7024df198736472dd50ca970eb22796082e3d489577')
{'ResponseMetadata': {'RequestId': 'f8341d4e-4047-11e7-9e70-afa8d465756c', 'HTTPStatusCode': 200, 'HTTPHeaders': {'x-amzn-requestid': 'f234564e-1234-56e7-89e7-a10fa45t789c', 'cache-control': 'private, no-cache, no-store, max-age=0', 'content-type': 'application/json', 'content-length': '733', 'date': 'Wed, 24 May 2017 06:12:30 GMT'}, 'RetryAttempts': 0}, 'Metadata': {'Id': 'abc123def456ghi789jkl789mno4be7024df198736472dd50ca970eb22796082e3d489577', 'Name': 'sentences', 'CreatorId': 'S-1-5-21-2125721135-1643952666-3011040551-2105&d-906724f1ce', 'ParentFolderId': '0a811a922403ae8e1d3c180f4975f38f94372c3d6a2656c50851c7fb76677363', 'CreatedTimestamp': datetime.datetime(2017, 5, 23, 12, 59, 13, 8000, tzinfo=tzlocal()), 'ModifiedTimestamp': datetime.datetime(2017, 5, 23, 13, 13, 9, 565000, tzinfo=tzlocal()), 'ResourceState': 'ACTIVE', 'Signature': 'b7f54963d60ae1d6b9ded476f5d20511'}}
```

## Create an application<a name="wd-app-create-app"></a>

As an Amazon WorkDocs administrator, create your application using the following steps\.

**To create an application**

1. Open the Amazon WorkDocs console at [https://console\.aws\.amazon\.com/zocalo/](https://console.aws.amazon.com/zocalo/)\.

1. Choose **My Applications**, **Create an Application**\.

1. Enter the following values:  
**Application Name**  
Name for the application\.  
**Email**  
Email address to associate with the application\.  
**Application Description**  
Description for the application\.  
**Redirect URIs**  
The location that you want Amazon WorkDocs to redirect traffic to\.  
**Application Scopes**  
The scope, either read or write, that you wish your application to have\. For more details, see [Application scopes](#wd-app-scopes)\.

1. Choose **Create**\.

## Application scopes<a name="wd-app-scopes"></a>

Amazon WorkDocs supports the following application scopes:
+ Content Read \(`workdocs.content.read`\), which gives your application access to the following Amazon WorkDocs APIs:
  + Get\*
  + Describe\*
+ Content Write \(`workdocs.content.write`\), which gives your application access to the following Amazon WorkDocs APIs:
  + Create\*
  + Update\*
  + Delete\*
  + Initiate\*
  + Abort\*
  + Add\*
  + Remove\*

## Authorization<a name="wd-authorization"></a>

After application registration is complete, an application can request authorization on behalf of any Amazon WorkDocs user\. To do this, the application should visit the Amazon WorkDocs OAuth endpoint, `https://auth.amazonworkdocs.com/oauth`, and provide the following query parameters:
+ \[Required\] `app_id`—Application ID generated when an application is registered\.
+ \[Required\] `auth_type`—The OAuth type for the request\. Supported value is `ImplicitGrant`\.
+ \[Required\] `redirect_uri`—The redirect URI registered for an application to receive an access token\.
+ \[Optional\] `scopes`—A comma\-deliminated list of scopes\. If not specified, the list of scopes selected during registration will be used\.
+ \[Optional\] `state`—A string which is returned along with an access token\.

**Note**  
If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

A sample GET request to initiate the OAuth flow to obtain an access token:

```
GET https://auth.amazonworkdocs.com/oauth?app_id=my-app-id&auth_type=ImplicitGrant&redirect_uri=https://myapp.com/callback&scopes=workdocs.content.read&state=xyz
```

The following takes place during the OAuth authorization flow:

1. The application user is prompted to enter the Amazon WorkDocs site name\.

1. The user is redirected to the Amazon WorkDocs authentication page to enter their credentials\.

1. After successful authentication, the user is presented with the consent screen that allows the user to either grant or deny your application the authorization to access Amazon WorkDocs\.

1. After the user chooses `Accept` on the consent screen, their browser is redirected to your application's callback URL along with the access token and region information as query parameters\.

A sample GET request from Amazon WorkDocs:

```
GET https://myapp.com/callback?acessToken=accesstoken&region=us-east-1&state=xyz
```

In addition to the access token, the Amazon WorkDocs OAuth service also returns `region` as a query parameter for the selected Amazon WorkDocs site\. External applications should use the `region` parameter to determine the Amazon WorkDocs service endpoint\.

If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

## Invoking Amazon WorkDocs APIs<a name="wd-auth-invoke"></a>

After obtaining the access token, your application can make API calls to Amazon WorkDocs services\.

**Important**  


This example shows how to use a curl GET request to obtain a document's metadata\.

```
Curl "https://workdocs.us-east-1.amazonaws.com/api/v1/documents/{document-id}" -H "Accept: application/json" -H "Authentication: Bearer accesstoken"
```

A sample JavaScript function to describe a user's root folders:

```
function printRootFolders(accessToken, siteRegion) {
    var workdocs = new AWS.WorkDocs({region: siteRegion});
    workdocs.makeUnauthenticatedRequest("describeRootFolders", {AuthenticationToken: accessToken}, function (err, folders) {
        if (err) console.log(err);
        else console.log(folders);
    });     
}
```

A sample Java\-based API invocation is described below:

```
AWSCredentialsProvider credentialsProvider = new AWSCredentialsProvider() {
  @Override
  public void refresh() {}

  @Override
  public AWSCredentials getCredentials() {
    new AnonymousAWSCredentials();
  }
};

// Set the correct region obtained during OAuth flow.
workDocs =
    AmazonWorkDocsClient.builder().withCredentials(credentialsProvider)
        .withRegion(Regions.US_EAST_1).build();

DescribeRootFoldersRequest request = new DescribeRootFoldersRequest();
request.setAuthenticationToken("access-token-obtained-through-workdocs-oauth");
DescribeRootFoldersResult result = workDocs.describeRootFolders(request);

for (FolderMetadata folder : result.getFolders()) {
  System.out.printf("Folder name=%s, Id=%s \n", folder.getName(), folder.getId());
}
```