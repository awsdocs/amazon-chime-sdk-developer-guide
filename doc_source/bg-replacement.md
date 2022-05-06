# Using background replacement<a name="bg-replacement"></a>

The background filter API allows you to replace blurred backgrounds with another type of background, such as an image\. To add a background replacement filter in a video stream, you need to create a `VideoFrameProcessor` using `BackgroundReplacementVideoFrameProcessor`, and then insert that processor into a `VideoTransformDevice`\.

Background replacement is integrated into the browser demo application\. To try it out, **enter npm run start** to launch the demo, join the meeting, choose the camera icon to enable video, then open the video filter list and choose **Background Replacement**\.

## Checking for browser support<a name="browser-support"></a>

Some browsers support the Amazon Chime SDK but do not support a background filter\. Also, some devices can't keep up with real\-time video while filtering a background\. The SDK provides an asynchronous static method that checks for the required browser features\.

```
import { BackgroundReplacementVideoFrameProcessor } from 'amazon-chime-sdk-js';
await BackgroundReplacementVideoFrameProcessor.isSupported();
```

If `isSupported` resolves to `true`, instantiate the background filter video frame processor\.

```
const processors = [];
if (await BackgroundReplacementVideoFrameProcessor.isSupported()) {
    const replacementProcessor = await BackgroundReplacementVideoFrameProcessor.create();
    processors.push(replacementProcessor);
}
```

The create method pre\-loads the model file and prepares a background filter for use\. Failure to load necessary resources results in `isSupported` returning `false`\. When you create a processor that a browser doesn't support, a no\-op video frame processor is returned and a background filter isn't applied\. In order to reduce the time to start a meeting, we recommend creating the background filter before you start the meeting\.

## Adding a background filter<a name="add-filter"></a>

A background filter integrates with the SDK by implementing a new `VideoFrameProcessor`\. After you check for support and create a background processor, you add the processor to the transform device\.

```
const processors = [];
if (await BackgroundReplacementVideoFrameProcessor.isSupported()) {
    const image = await fetch('https://pathtoimage.jpeg'); 
    const imageBlob = await image.blob();
    const options = { imageBlob };
    const replacementProcessor = await BackgroundReplacementVideoFrameProcessor.create(null, options); 
    processors.push(replacementProcessor);
}
let transformDevice = new DefaultVideoTransformDevice(logger, device, processors);
```

## Setting replacement options<a name="options"></a>

You can set a number of options for a background replacement filter:
+ `logger`: A logger to which log output is written\.
+ `reportingPeriodMillis`: The frequency with which the video frame processor reports observed events\.
+ `filterCPUUtilization`: The threshold CPU utilization for skipping background filter updates\. For more about how and why the SDK skips updates, see the next section\. This reduces the amount of CPU used by a background filter\. For example, If the reporting period is set to 1000 ms, and 500 ms is dedicated to processing the background filter, then the CPU utilization for that reporting period is 50 percent\. If you set `filterCPUUtilization` to 50, the `BackgroundReplacementVideoFrameProcessorObserver` fires a `filterCPUUtilizationHigh` event\.
+ `imageBlob`: A blob that contains the image used in the background\. If you don't provide an image blob, a default solid blue will be displayed\. The Blob can be created in many different ways, such as https fetch, file upload, and so on\. If you use fetch to load the image blob, we recommend using HTTPS for security reasons\.

This example shows a typical set of options\.

```
 interface BackgroundReplacementOptions {
  logger?: Logger;
  reportingPeriodMillis?: number;
  filterCPUUtilization?: number;
  imageBlob?: Blob;
}
```

## Mitigating CPU usage<a name="cpu-mitigation"></a>

The `BackgroundReplacementOptions` interface has a `filterCPUUtilization` field that allows you to configure the amount of CPU utilized by the background filter processor\. The Amazon Chime SDK for JavaScript uses this field to determine when to fire the `filterCPUUtilizationHigh` event on the `BackgroundReplacementVideoFrameProcessorObserver`\.

 The SDK also uses the event internally to determine if the `filterCPUUtilization` threshold is being exceeded\. To mitigate excessive CPU usage, the SDK begins to skip background segmentation until the CPU utilization is at or below the configured percentage set in the filterCPUUtilization field\. Lower CPU utilization increases amount of skipped background segmenting while maintaining the desired CPU levels\. Higher utilization reduces skipped background segmenting, but increases CPU usage\.

## Using observer notifications<a name="observer-notifications"></a>

You can optionally implement the `BackgroundReplacementVideoFrameProcessorObserver` interface and use `addObserver` to receive callbacks when an event occurs:
+ `filterFrameDurationHigh`: This event occurs when the amount of time it takes to apply the background filter is longer than expected\. The measurement is taken from the time the process method starts to when it returns\. For example, if the video runs at 15 frames per seconds and you average more than 67 ms \(1000 ms reporting period / 15 fps\) to apply a background filter, then a large portion of each frame's maximum processing time is taken up by processing a background filter\.
+ f`ilterCPUUtilizationHigh`: This event occurs when the CPU utilization of a background filter exceeds the `filterCPUUtilization` threshold defined in the `BackgroundReplacementOptions`\. You can use this event as a trigger to disable a background filter when CPU utilization is too high\.