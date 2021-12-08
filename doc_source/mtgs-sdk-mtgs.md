# Amazon Chime SDK meetings and AWS Regions<a name="mtgs-sdk-mtgs"></a>

Amazon Chime SDK meetings have *control regions* and *media regions*\. A control region provides the API endpoint used to create, update, and delete meetings\. Media regions host the actual meetings\.

Typically, your application service uses the [AWS SDK](https://aws.amazon.com/tools/) to [sign](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) and call APIs in the control plane, and your application client uses the [Amazon Chime SDK for Javascript](https://docs.aws.amazon.com/chime/latest/dg/use-javascript-sdk-top.html), [iOS](https://docs.aws.amazon.com/chime/latest/dg/sdk-for-ios.html), or [Android](https://docs.aws.amazon.com/chime/latest/dg/sdk-for-android.html) to connect to the meeting in the media plane\.

A control region can create a meeting in any media region\. However, you can only update a meeting in the control region used to create the meeting\. [Meeting events](https://docs.aws.amazon.com/chime/latest/ag/automating-chime-with-cloudwatch-events.html#sdk-events) such as `AttendeeJoined` are sent to [EventBridge, Amazon Simple Queue Service \(SQS\) and/or Amazon Simple Notification Service \(SNS\)](https://docs.aws.amazon.com/chime/latest/dg/mtgs-sdk-notifications.html) in the meeting control region\.

 For a list of available Amazon Chime SDK meeting control and media regions, refer to [Available regions](sdk-available-regions.md) in this guide\.

This diagram shows the typical flow of data through the control and media regions\.

![\[Diagram showing the flow of data through the Amazon Chime control and media planes.\]](http://docs.aws.amazon.com/chime/latest/dg/images/control-media-regions.png)

**Topics**
+ [Migrating to the Amazon Chime SDK Meetings namespace](meeting-namespace-migration.md)
+ [Meeting Regions](chime-sdk-meetings-regions.md)
+ [Creating Meetings](create-mtgs.md)
+ [Choosing a media Region](#choose-chime-sdk-media-region)
+ [Choosing the nearest media Region](#choose-chime-sdk-nearest-media-region)
+ [Network configuration](network-config.md)
+ [Meeting events](using-events.md)
+ [Creating Amazon Chime media capture pipelines](media-capture.md)
+ [Using Amazon Chime SDK live transcription](meeting-transcription.md)

## Choosing a media Region<a name="choose-chime-sdk-media-region"></a>

When choosing a media Region to use for your Amazon Chime SDK meeting, common factors to consider include the following:

**Regulatory requirements**  
If your Amazon Chime SDK meetings are subject to regulations requiring them to be hosted within a geopolitical border, consider hardcoding the meeting Region based on fixed application logic\.  
For example, a telemedicine application might require all meetings to be hosted within the medical practitioner's jurisdiction\. If the application supports clinics located in both Europe and the United States, you can use each clinic's address to select a Region within its jurisdiction\.

**Meeting quality**  
Specifying a Region for your Amazon Chime SDK meeting can help enhance the meeting quality for your attendees, whether they are located near each other or distributed geographically\. However, when you host an Amazon Chime SDK meeting in a media Region, each attendee's audio and video is sent and received from that Region\. As the distance between the attendee and the Region increases, network latency can reduce meeting quality\. 

You can use one of the following methods to choose a media Region for your Amazon Chime SDK meeting:

**Hardcode a media Region**  
Recommended if your Amazon Chime SDK meetings are all hosted within a specific AWS Region\.

**Choose the nearest media Region**  
Recommended if your Amazon Chime SDK meeting attendees are located in the same AWS Region, but your meetings are hosted in different Regions\.

For more details about choosing the nearest media Region, see the following topic\.

## Choosing the nearest media Region<a name="choose-chime-sdk-nearest-media-region"></a>

Call `https://nearest-media-region.l.chime.aws` to identify the nearest media Region for hosting your Amazon Chime SDK meeting\. Make the call from the client application, not the server application\. Pass the call to the application before your attendees need to join the meeting, such as when the application starts\. Your request returns a JSON object that shows the nearest AWS Region\.

The following example shows the contents of a request sent to `https://nearest-media-region.l.chime.aws` to identify the nearest media Region\.

```
async getNearestMediaRegion(): Promise<string> {
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