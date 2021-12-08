# Meeting Regions<a name="chime-sdk-meetings-regions"></a>

Amazon Chime SDK meetings have *control regions* and *media regions*\. A control region has an API endpoint used to create, update and delete meetings, and media regions host the actual meetings\.

Typically, your application service uses the [AWS SDK](https://aws.amazon.com/tools/) to [sign and call](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) APIs in the control plane, and your application client uses the [Amazon Chime SDK for Javascript](https://docs.aws.amazon.com/chime/latest/dg/use-javascript-sdk-top.html), [iOS](https://docs.aws.amazon.com/chime/latest/dg/sdk-for-ios.html), or [Android](https://docs.aws.amazon.com/chime/latest/dg/sdk-for-android.html) to connect to the meeting in the media plane\.

A control region can create a meeting in any media region\. However, you can only update a meeting in the control region used to create it\. Meeting [events](https://docs.aws.amazon.com/chime/latest/ag/automating-chime-with-cloudwatch-events.html#sdk-events) such as `AttendeeJoined` go to [EventBridge, Amazon Simple Queue Service \(SQS\), or Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/chime/latest/dg/mtgs-sdk-notifications.html) in the meeting control region\.

 For a list of available Amazon Chime SDK meeting control and media regions, refer to [Available regions](sdk-available-regions.md) in this guide\.

## Choosing a control region<a name="choose-meeting-region"></a>

Remember these factors when choosing a control Region for an Amazon Chime SDK meeting:
+ **Regulatory requirements**\. Is your application requiried to be within a geopolitical border, or use an endpoint with FIPS 140\-2 validated cryptographic modules?
+ **API latency**\. Using the control Region nearest to the AWS Region of your application service can help reduce the APIs' network latency\. In turn, that helps reduce the time needed to create meetings, and let users join meetings faster\.
+ **High Availability**\. You can use multiple control Regions to implement a high availability architectures\. However each control Region operates independently\. Also, you can only update meetings in the control Region used to create them\. Futher, you must use that same region to consume meeting events with [ EventBridge, Amazon Simple Queue Service \(SQS\), or Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/chime/latest/dg/mtgs-sdk-notifications.html)\.

## Choosing a media region<a name="choose-media-region"></a>

**Note**  
We recommend that you always specify a value in the `MediaRegion` parameter in the [CreateMeeting](https://docs.aws.amazon.com/chime/latest/APIReference/API_CreateMeeting.html) API action\. For more information about the Regions, refer to [Available regions](sdk-available-regions.md)\.

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

## Choosing the nearest media Region<a name="choose-nearest-media-region"></a>

Call `https://nearest-media-region.l.chime.aws` to identify the nearest meeting Region that can host your Amazon Chime SDK meeting\. Make the call from the client application, not the server application\. Pass the call to the application before your attendees join the meeting, such as when the application starts\. Your request returns a JSON object that shows the nearest AWS Region\.

The following example shows the contents of a request sent to `https://nearest-media-region.l.chime.aws` to identify the nearest meeting Region\.

```
async getNearestMediaRegion(): Promisestring {
    var nearestMediaRegion = '';
    const defaultMediaRegion = 'us-east-1';
    try {
      const nearestMediaRegionResponse = await fetch(
        `https://nearest-media-region.l.chime.aws`,
        {
          method: 'GET',
        }
      );
      const nearestMediaRegionJSON = await nearestMediaRegionResponse.json();
      this.log(nearestMediaRegionJSON.region);
      nearestMediaRegion = nearestMediaRegionJSON.region;
    } catch (error) {
      nearestMediaRegion = defaultMediaRegion;
      this.log('Default media region ' + defaultMediaRegion + ' selected: ' + error.message);
    } finally {
      return nearestMediaRegion;
    }
  }
```