# Common issues<a name="common-issues"></a>

The following sections provide troubleshooting methods for common meeting issues\.

**Topics**
+ [Connectivity issues](#connectivity-issues)
+ [Audio and video quality issues](#a-v-quality)
+ [Veryifing SDK quotas and API throttling](quotas-throttling.md)
+ [Opening support cases](open-support-cases.md)

## Connectivity issues<a name="connectivity-issues"></a>

For connectivity issues, see [Verifying network access](self-troubleshooting.md#net-acess)\.

## Audio and video quality issues<a name="a-v-quality"></a>

Audio and video quality issues can have several causes\. Two main reasons for sub optimal audio/video quality are network bandwidth, and device performance\. For detailed information on different challenges and how these impact audio/video quality, review Quality, Bandwidth, and Connectivity \(https://aws\.github\.io/amazon\-chime\-sdk\-js/modules/qualitybandwidth\_connectivity\.html\)\. This article describes different events and metrics that can be monitored to detect bandwidth issues and potential mitigations\. 

You can choose a Media Region that is closer to the audience of the target meeting session\. To understand how to choose an optimal media region, see Using meeting Regions \(https://docs\.aws\.amazon\.com/chime\-sdk/latest/dg/chime\-sdk\-meetings\-regions\.html\)\.

Depending on the bandwidth available for a meeting attendee, the Amazon Chime SDK adjusts the video quality of the video being received/uploaded\. To understand how you can control the video quality for different video layouts, visit Managing Video Quality for different Video Layouts \(https://aws\.github\.io/amazon\-chime\-sdk\-js/modules/videolayout\.html\)\. This article describes video lifecycle management and uplink/downlink policies\. 

**Video resolution considerations**
+ Default resolution for uploading video is 540p and 15fps at 1400 kbps\. Depending on bandwidth, you can reduce that resolution and frame rate\.
+ Based on receiver bandwidth available, determine how many video tiles to show\. Don’t overshoot 6Mbps for all video tiles and content sharing\. End users see black video tiles when they don't have enough bandwidth\.

**Using video uplink and downlink bandwith policies**  
The Amazon Chime SDK provides the following bandwidth policies\.
+ NScaleVideoUplinkBandwidthPolicy – Implements capture and encoding parameters that nearly equal those used by the desktop, web, and mobile clients\.
+ AllHighestVideoBandwidthPolicy – Always subscribes to the highest quality video stream\.
+ NoVideoDownlinkBandwidthPolicy – Disables video when bandwidth drops below a given threshold\.
+ VideoPriorityBasedPolicy – Prioritizes audio over video in cases of low bandwidth\.
+ VideoAdaptiveProbePolicy