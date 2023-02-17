# Using AppKeys and TenantIDs<a name="app-keys-tenant-ids"></a>

You can use AppKeys and TenantIDs to limit access *from a network* to specific applications' Amazon Chime SDK WebRTC media sessions\.

Developers use the Amazon Chime SDK to create applications that send and receive real\-time video over UDP\. Application users require UDP access to the [https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) subnet\. Organizations \(network owners\) can use AppKeys and TenantIDs to limit access from their network to only a specific application's WebRTC media sessions\.

**Example 1: Using AppKeys**  
If App\-A and App\-B use the Amazon Chime SDK an organization can allow App\-A to access the WebRTC media sessions from their network, but block App\-B and any other applications that use the Amazon Chime SDK\. Organizations can do that with App\-A's AppKey and an HTTPS proxy\. For more information, refer to [Limiting access to a specific application](#limit-app-access), later in this topic\.

**Example 2: Using AppKeys and TenantIDs**  
If App\-A is publicly available and used by many customers, an organization may want to allow App\-A to access WebRTC media sessions from their network only when their users are part of the session, and block access to all other App\-A sessions\. Organizations can do this by using the application's AppKey, the organization's TenantID, and an HTTPS proxy\. For more information, refer to [Limiting access to a specific tenant](#limit-tenant-access), later in this topic\.

To use AppKeys and TenantIDs, you must have an HTTPS proxy server that allows adding HTTPS headers to a request\. The following diagram shows how AppKeys and TenantIDs work\.

![\[Diagram showing how AppKeys and TenantIDs control application and tenant access to a WebRTC session.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/app-key-diagram.png)

In the image, App\-A has tenants A\-1 and A\-2, and App\-B has tenants B\-1 and B\-2\. In this case, the AppKey only allows App\-A to connect to the WebRTC media session, and the tenant ID only admits Tenant A\-1 to the session\.

**Topics**
+ [Limiting access to a specific application](#limit-app-access)
+ [Limiting access to a specific tenant](#limit-tenant-access)
+ [HTTPS header examples](#header-examples)

## Limiting access to a specific application<a name="limit-app-access"></a>

An *AppKey* is a consistent, unique 256\-bit value that Amazon Chime creates for each AWS account\. If you don't have an AppKey, you can request one from Amazon Support\. If you have multiple AWS accounts, you can request a common AppKey for all your accounts\.

**Note**  
You can safely share your AppKeys publicly and enable other organizations to limit access from their networks\. 

The Amazon Chime SDK automatically associates each WebRTC media sessions with an AppKey based on the AWS account ID used to create the session\. To limit access *from your network* to specific applications, do the following:

1. Route all outbound requests to the `CHIME_MEETINGS` subnet through an HTTPS proxy server\. 

1. Configure the proxy server to add the following header to all outbound requests to the `CHIME_MEETINGS` subnet:

   `X-Amzn-Chime-App-Keys:` *comma\-separated list of allowed AppKeys*\.

   For example, `X-Amzn-Chime-App-Keys: AppKey1,AppKey2,AppKey3` allows the apps associated with those AppKeys to access the subnet\.

The Amazon Chime SDK inspects inbound WebRTC media session connections for the `X-Amzn-Chime-App-Keys` header and applies the following logic:

1. If the `X-Amzn-Chime-App-Keys` header is present and includes the session's AppKey, accept the connection\.

1. If the `X-Amzn-Chime-App-Keys` header is present but does not include the session's AppKey, reject the connection with a 403 error\.

1. If the `X-Amzn-Chime-App-Keys` header is not present, accept the connection\. If users can access the application from outside the organization's network, they can also access the session\.

## Limiting access to a specific tenant<a name="limit-tenant-access"></a>

A *TenantID* is an opaque identifier created by developers\. Remember the following about TenantIDs:
+ TenantIDs are not guaranteed to be unique between applications, so you must specify an AppKey for each TenantID list\. 
+ TenantIDs are case senstitive\. Enter them exactly as prescribed by the developer\.
+ An organization can limit access to multiple applications, but only specify TenantIDs for some of those applications\. Applications without TenantIDs can connect to all WebRTC media sessions\. 

To associate a media session with TenantIDs, a developer must first add the `TenantIds` property and a list of TenantIDs to a [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_meeting-chime_CreateMeeting.html) or [CreateMeetingWithAttendees](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_meeting-chime_CreateMeetingWithAttendees.html) request\.

For example:

`CreateMeeting(..., TenantIds : [ tenantId1, tenantId2 ] )`

To limit access from an organization's network to their WebRTC media session in specific applications, do the following:

1. Follow the steps in [Limiting access to a specific application](#limit-app-access)\.

1. Configure the HTTPS proxy server to add an `X-Amzn-Chime-Tenants` header on outbound connections\. Include a list of AppKeys and TenantIDs delimited by semicolons\. For example: `X-Amzn-Chime-Tenants: AppKey1;tenantId-1,tenantId-2;AppKey2:tenantId-1,tenantId-1`

The Amazon Chime SDK inspects inbound WebRTC media session connections for the `X-Amzn-Chime-Tenants` header and applies the following logic:
+ If the header includes the session's `AppKey:tenantId`, accept the connection\.
+ If the header includes the session's `AppKey` but no matching `tenantId`, reject the connection with a 403 error\.
+ If the header does *not* include the session's `AppKey`, accept the connection\.
+ If the header includes the session's `AppKey`, but the session doesn't have at least one allowed `tenantId`, reject the connection with a 403 error\. This may be a developer bug\.
+ If the header is not present, accept the connection\. If users can access the application from outside the organization's network, they can also access all sessions\.

## HTTPS header examples<a name="header-examples"></a>

The following examples show some of the ways to use AppKeys and TenantIDs in HTTPS headers\.

**One app with one tenant**  
`X-Amzn-Chime-App-Keys: AppKey`  
`X-Amzn-Chime-Tenants: AppKey:orgId`  
Users can access only the organization's WebRTC media sessions in the specified app\. All other apps are blocked\.

**One app with two tenants**  
`X-Amzn-Chime-App-Keys: AppKey`  
`X-Amzn-Chime-Tenants: AppKey:engineeringId,salesId`  
Users can access only the media sessions for engineering and sales in the specified app\. All other apps are blocked\.

**Two Apps, One limited to a Tenant**  
`X-Amzn-Chime-App-Keys: AppKey1,AppKey2`  
`X-Amzn-Chime-Tenants: AppKey1:orgId`  
Users can access only the organization's media sessions in App 1, and any session in App 2\. All other apps are blocked\.