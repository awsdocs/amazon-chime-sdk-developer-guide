# Migrating to the ChimeSdkMediaPipelines namespace<a name="migrate-pipelines"></a>

You use the `ChimeSdkMediaPipelines` namespace to address media pipeline API endpoints in any Region in which they're available\. Use this namespace if you're just starting to use the Amazon Chime SDK\. 

Existing applications that use the [Amazon Chime](https://docs.aws.amazon.com/chime/latest/APIReference/API_Operations_Amazon_Chime.html) namespace can continue to do so, but to use the updated APIs and new features, you must migrate to the new namespace\.

**Topics**
+ [Reasons to migrate your pipelines](#pipeline-migration-reasons)
+ [Before you migrate your pipelines](#migration-prerequisites)

## Reasons to migrate your pipelines<a name="pipeline-migration-reasons"></a>

We encourage you to migrate to the media pipeline namespace for these reasons:

**Choice of API Endpoint**  
The Amazon Chime SDK Media Capture namespace is the only API namespace which can use API endpoints in any Region that makes them available\. For more information about Regions, refer to [Available regions](sdk-available-regions.md)\. If you want to use API endpoints other than us\-east\-1, you must use the `ChimeSdkMediaPipelines` namespace\.

**Updated and new media pipeline APIs**  
We only add or update media pipeline APIs in the `ChimeSdkMediaPipelines` namespace\. We fully support the current media capture APIs in the `Chime` namespace, but they remain as\-is\.

## Before you migrate your pipelines<a name="migration-prerequisites"></a>

Before you migrate, be aware of the differences between the namespaces\. The following table lists and describes them\.


| Item | Media pipelines namespace | Chime namespace | 
| --- | --- | --- | 
|  Namespace names  |  ChimeSdkMediaPipelines  |  Chime  | 
|  Regions  |  Multiple  |  us\-east\-1 only  | 
|  Endpoints  |  https://media\-pipelines\-chime\.*region*\.amazonaws\.com  |  https://service\.chime\.aws\.amazon\.com  | 
|  Service principal  |  mediapipelines\.chime\.amazonaws\.com  |  chime\.amazonaws\.com  | 
|  APIs  |  Only APIs for media pipelines  |  APIs for meetings and other parts of Amazon Chime  | 
|  Meetings  |  Media pipelines in the us\-west\-2, ap\-southeast\-1, and eu\-central\-1 regions only work with meetings created in the Amazon Chime SDK Meetings namespace\. Media pipelines in the us\-east\-1 region work with meetings created by any meeting endpoint in either namespace\.  |  Media pipelines work with meetings created by any meetings endpoint in either namespace\.  | 
| Default active media pipelines | 100 in the us\-east\-1 Region, and 10 in the us\-west\-2, ap\-southeast\-1, and eu\-central\-1 Regions\.  | 100 in us\-east\-1 only\. | 
|  Service\-linked role  |  AWSServiceRoleForAmazonChimeSDKMediaPipelines  |     | 
|  Tags  |  Available  |  Not available for the media pipeline APIs\.  | 
| CloudTrail event source | chime\-sdk\-media\-pipelines\.amazonaws\.com | chime\.amazonaws\.com\. | 
| Media live connector | Available |  Not available for the media pipeline APIs\.  | 
| Compositing | Available |  Not available for the media pipeline APIs\.  | 
| Concatenation | Available | Not available\. | 

The following list provides more information about the differences between the Chime and AWSChimeSdkMediaPipelines namespaces\.

**Namespace names**  
The Amazon Chime SDK namespace uses the `AWS.Chime` formal name\. The Amazon Chime SDK Media Pipelines namespace uses the `AWS.ChimeSDKMediaPipelines` formal name\. The precise format of the name varies by platform\.  
For example, this line of Node\.js code addresses the `chime` namespace:  

```
const chimeMediaPipelines = AWS.Chime();
```
To migrate to the Media Pipelines SDK namespace, update that code with the new namespace and the endpoint region\.  

```
const chimeMediaPipelines = AWS.ChimeSDKMediaPipelines({ region: "eu-central-1" });
```

**Regions**  
The Amazon Chime namespace only addresses API endpoints in the US\-EAST\-1 region\. The Amazon Chime SDK Media Pipelines namespace addresses Amazon Chime SDK media pipeline API endpoints in any Region that has them\. For a current list of media pipeline Regions, see [Available regions](sdk-available-regions.md) in this guide\.

**Endpoints**  
To modify a media capture pipeline, you must use the same endpoint that you created the pipeline in\. For example, if you created pipelines via an endpoint in eu\-central\-1, you must use eu\-central\-1 to interact with that pipeline\.

**Service principal**  
The [Amazon Chime SDK Media Pipelines](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_Operations_Amazon_Chime_SDK_Meetings.html) namespace uses a new service principal: `mediapipelines.chime.amazonaws.com`\. If you have Amazon S3 bucket or other IAM policies that grant access to services, you need to update those polices to grant access to the new service principal\.  
For example, when you create a media pipelines, you must add the policy permissions listed in [Creating an Amazon S3 bucket](create-s3-bucket.md) to the new service principal\. For more information about policies, see [ AWS JSON policy elements: Principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) in the IAM User Guide\.

**APIs**  
The Amazon Chime SDK Media Pipelines namespace only contains APIs that create and manage media pipelines\. The Amazon Chime namespace includes APIs for media pipelines, meetings, and other parts of the Amazon Chime service\.

**Meetings**  
Media pipelines in the IAD region work with meetings created by any meetings endpoint with either namespace\.

**Service\-linked role**  
Only for the Amazon Chime SDK Media Pipelines namespace\. Create the *AWSServiceRoleForAmazonChimeSDKMediaPipelines* role\.

**Tags**  
The [Amazon Chime SDK Media Pipelines](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_Operations_Amazon_Chime_SDK_Meetings.html) namespace supports tags\. The role must have permission to call the `TagResource` operation when calling the [CreateMediaCapturePipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaCapturePipeline.html) or [CreateMediaLiveConnectorPipeline](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_media-pipelines-chime_CreateMediaLiveConnectorPipeline.html) APIs with one or more tags\.