# Using meeting Regions<a name="chime-sdk-meetings-regions"></a>

Amazon Chime SDK meetings have *control* Regions and *media* Regions\. Control Regions have an API endpoint used to create, update, and delete meetings\. Media Regions host the actual meetings\.

Typically, your application service uses the [AWS SDK](https://aws.amazon.com/tools/) to [sign and call](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) APIs in control Regions\. Your application client uses the Amazon Chime SDK client libraries for [JavaScript](js-sdk-intro.md), [iOS](sdk-for-ios.md), or [Android](sdk-for-android.md) to connect to the meeting in media Regions\.

A control region can create a meeting in any media Region in the same AWS partition\. However, you can only update a meeting in the control region used to create it\. To find the media Region closest to a customer, call [https://nearest\-media\-region\.l\.chime\.aws](https://nearest-media-region.l.chime.aws)\.

Meeting [events](https://docs.aws.amazon.com/chime-sdk/latest/ag/automating-chime-with-cloudwatch-events.html#sdk-events) such as `AttendeeJoined` call [EventBridge, Amazon Simple Queue Service \(SQS\), or Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/chime-sdk/latest/dg/mtgs-sdk-notifications.html) in the meeting control Region\.

 For a list of available Amazon Chime SDK meeting control and media Regions, refer to [Available regions](sdk-available-regions.md) in this guide\.

This diagram shows the typical flow of data through the control and media Regions\.

![\[Diagram showing the flow of data through the Amazon Chime control and media Regions.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/control-media-regions.png)

## Choosing a control region<a name="choose-meeting-region"></a>

Remember these factors when choosing a control Region for an Amazon Chime SDK meeting:
+ **Regulatory requirements**\. Is your application requiried to be within a geopolitical border, or use an endpoint with FIPS 140\-2 validated cryptographic modules?
+ **API latency**\. Using the control Region nearest to the AWS Region of your application service can help reduce the APIs' network latency\. In turn, that helps reduce the time needed to create meetings, and let users join meetings faster\.
+ **High Availability**\. You can use multiple control Regions to implement a high availability architectures\. However each control Region operates independently\. Also, you can only update meetings in the control Region used to create them\. Futher, you must use that same region to consume meeting events with [ EventBridge, Amazon Simple Queue Service \(SQS\), or Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/chime-sdk/latest/dg/mtgs-sdk-notifications.html)\.

## Choosing a media region<a name="choose-media-region"></a>

**Note**  
We recommend that you always specify a value in the `MediaRegion` parameter in the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_CreateMeeting.html) API action\. For more information about the Regions, refer to [Available regions](sdk-available-regions.md)\.

When choosing a media Region to use for your Amazon Chime SDK meeting, common factors to consider include the following:

**Regulatory requirements**  
If your Amazon Chime SDK meetings are subject to regulations requiring them to be hosted within a geopolitical border, consider hardcoding the meeting Region based on fixed application logic\.  
For example, a telemedicine application might require all meetings to be hosted within the medical practitioner's jurisdiction\. If the application supports clinics located in both Europe and the United States, you can use each clinic's address to select a Region within its jurisdiction\. 

**Meeting quality**  
When an Amazon Chime SDK meeting is hosted in a media Region, each attendee's audio and video is sent and received from that Region\. As the distance between the attendee and the Region increases, meeting quality can be affected by network latency\. Specifying a Region for your Amazon Chime SDK meeting can help enhance the meeting quality for your attendees, whether they are located near each other or distributed geographically\.

You can use one of the following methods to choose a media Region for your Amazon Chime SDK meeting:

**Hardcode a media Region**  
Recommended if your Amazon Chime SDK meetings are all hosted within a specific AWS Region\.

**Choose the nearest media Region**  
Recommended if your Amazon Chime SDK meeting attendees are located in the same AWS Region, but your meetings are hosted in different Regions\.

## Finding the nearest media Region<a name="choose-nearest-media-region"></a>

To find the nearest media Region capable of hosting an Amazon Chime SDK meeting, call [https://nearest\-media\-region\.l\.chime\.aws](https://nearest-media-region.l.chime.aws)\. This endpoint returns a single Region, such as `{"region": "us-west-2"}`\. Call the URL from your client application to identify the Region closest to the user, and then use the result in the `MediaRegion` parameter of the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_meeting-chime_CreateMeeting.html) API to create the meeting in that Region\.

You usually call the URL when the client application starts, or its network connection changes\. By predetermining the nearest Region, you avoid adding the call’s latency at the time of meeting creation\.

## Finding the nearest AWS GovCloud \(US\) media Region<a name="choose-gov-cloud-region"></a>

To find the nearest AWS GovCloud \(US\) Region that can host an Amazon Chime SDK meeting, call [https://nearest\-us\-gov\-media\-region\.l\.chime\.aws](https://nearest-us-gov-media-region.l.chime.aws)\. This endpoint returns the nearest region, such as `{"region": "us-gov-west-1"}`\. Call the URL from your client application to identify the AWS GovCloud \(US\) closest to the user, and use the result in the `MediaRegion` parameter of the [CreateMeeting](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_meeting-chime_CreateMeeting.html) API to create the meeting in that Region\.

You usually call the URL when the client application starts, or its network connection changes\. By predetermining the nearest Region, you avoid adding the call’s latency at the time of meeting creation\.

## JavaScript example<a name="region-javascript"></a>

The following example uses HTML and JavaScript to return the nearest media Region and AWS GovCloud \(US\) media Region\.

```
<html>
<head>
  <title>Amazon Chime SDK - Nearest Media Region</title>
  <script>

async function getNearestMediaRegion(partition)  {

    console.log('Nearest media region partition: ' + partition);

    const url = ('aws-us-gov' == partition) ? 'https://nearest-us-gov-media-region.l.chime.aws' : 'https://nearest-media-region.l.chime.aws';
    let result = ('aws-us-gov' == partition) ? 'us-gov-west-1' : 'us-west-2';

    try { //Find the nearest media region
        console.log('Nearest media region URL: ' + url);
        const response = await fetch(url, {method: 'GET'} );
        const body = await response.json();
        result = body.region;
    } catch (error) {
        console.log(error.message);
    } finally {
        console.log('Nearest media region found: ' + result);
        return result;
    }
}

async function findRegions(partition) {
  aws.innerText = await getNearestMediaRegion();
  awsusgov.innerText = await getNearestMediaRegion('aws-us-gov');
}
  </script>
</head>
<body>
  <h3>Nearest media region, by AWS partition</h3>
  <table>
    <tr><th>Partition</th><th>Media Region</th></tr>
    <tr><td>aws</td><td id="aws">Finding...</td></tr>
    <tr><td>aws-us-gov</td><td id="awsusgov">Finding...</td></tr>
  </table>
  <script>
    findRegions();
  </script>
</body>
</html>
```

## Checking Region status<a name="region-status"></a>

Call [https://region\.status\.chime\.aws/](https://region.status.chime.aws/) to retrieve the health of the Amazon Chime SDK service in each Region\. The result shows the recommended Regions\. If a media Region has a status other than **recommended**, the nearest media Region endpoint will not return that Region\.

The following example shows a typical result\.

```
{
  "MeetingsControlRegions": {
    "us-east-1": "recommended",
    "us-west-2": "recommended",
    "ap-southeast-1": "recommended",
    "eu-central-1": "recommended"
  },
  "MeetingsMediaRegions": {
    "af-south-1": "recommended",
    "ap-northeast-1": "recommended",
    "ap-northeast-2": "recommended",
    "ap-south-1": "recommended",
    "ap-southeast-1": "recommended",
    "ap-southeast-2": "recommended",
    "ca-central-1": "recommended",
    "eu-central-1": "recommended",
    "eu-north-1": "recommended",
    "eu-south-1": "recommended",
    "eu-west-1": "recommended",
    "eu-west-2": "recommended",
    "eu-west-3": "recommended",
    "sa-east-1": "recommended",
    "us-east-1": "recommended",
    "us-west-2": "recommended"
  },
  "MediaPipelineControlRegions": {
    "ap-southeast-1": "recommended",
    "eu-central-1": "recommended",
    "us-east-1": "recommended",
    "us-west-2": "recommended"
  },
  "MediaPipelineDataRegions": {
    "af-south-1": "recommended",
    "ap-northeast-1": "recommended",
    "ap-northeast-2": "recommended",
    "ap-south-1": "recommended",
    "ap-southeast-1": "recommended",
    "ap-southeast-2": "recommended",
    "ca-central-1": "recommended",
    "eu-central-1": "recommended",
    "eu-north-1": "recommended",
    "eu-south-1": "recommended",
    "eu-west-1": "recommended",
    "eu-west-2": "recommended",
    "eu-west-3": "recommended",
    "sa-east-1": "recommended",
    "us-east-1": "recommended",
    "us-west-2": "recommended"
  }
}
```