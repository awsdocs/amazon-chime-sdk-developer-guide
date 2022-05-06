# Using background blur<a name="background-blur"></a>

You can use video background blur to obfuscate your users’ surroundings, which can help increase visual privacy\. Video background blur runs locally in each user's browser, transforming video before it is shared into the meeting\. Users can confirm their background is blurred prior to joining a meeting through video preview\. Users can also adjust the blur strength, from a low\-strength bokeh effect to high\-strength obfuscation\.

The background blur API allows builders to enable background blurring on an incoming video stream\. To add blurring to a video stream, you need to create a `VideoFrameProcessor` using `BackgroundBlurVideoFrameProcessor`, and then insert that processor into a `VideoTransformDevice`\.

The video frame processor uses a TensorFlow Lite machine learning model, plus JavaScript Web Workers and WebAssembly to apply blur to the background of each frame in a video stream\. The SDK downloads those assets at runtime when the video frame processor is created\. You can customize the download location, and you can host the assets on a content delivery network\. The assets don't reside in the [sample application on GitHub](https://github.com/aws/amazon-chime-sdk-js/)\. 

**Topics**
+ [Creating a processor and adding it to a pipeline](#create-processor)
+ [Using a custom model for background segmentation](#segmentation-model)
+ [Observing processor events](#processor-events)

## Creating a processor and adding it to a pipeline<a name="create-processor"></a>

To create a background blur frame processor with the default assets and options, you use the `BackgroundBlurVideoFrameProcessor` class\. Use the \.isSupported method to ensure that the processor is supported in its runtime environment\. If it is supported, then create the processor and use it in the `VideoTransformDevice`\.

This example shows how to create a default processor, ensure it's supported, and use it in a `VideoTransformDevice`\.

```
const processors = [];
// create blur processor
if (BackgroundBlurVideoFrameProcessor.isSupported()) {
    const blurProcessor = await BackgroundBlurVideoFrameProcessor.create();
    processors.push(blurProcessor);
}

// add the processor to the pipeline
let transformDevice = new DefaultVideoTransformDevice(logger, device, processors);
```

You can customize the video frame processor by pass ing in a `BackgroundFilterSpec` object and a `BackgroundFilterOptions` object\. The `BackgroundFilterSpec` class allows you to customize the path to an alternate content delivery network, and use a custom machine learning model for background segmentation\. The `BackgroundBlurOptions` class allows you to customize the behavior of the processor as follows:
+ `logger`: A logger to which log output is written\.
+ `filterCPUUtilization`: The threshold CPU utilization for skipping background filter updates\. For more about how and why the SDK skips updates, see [Using background replacement](bg-replacement.md), the next topic in this guide\. This reduces the amount of CPU used by a background filter\. For example, If the reporting period is set to 1000 ms, and 500 ms is dedicated to processing the background filter, then the CPU utilization for that reporting period is 50 percent\. If you set `filterCPUUtilization` to 50, the `BackgroundReplacementVideoFrameProcessorObserver` fires a `filterCPUUtilizationHigh` event\.
+ `blurStrength`: The amount of blurring applied to the video stream\. Must be a value greater than 0\. 
+ `reportingPeriodMillis`: The frequency with which the video frame processor reports observed events\.

This example shows some typical customizations\.

```
/**
 * An interface to define the paths for loading the Web Worker JavaScript, WebAssembly (WASM), 
 * and the definition and path of the model to use to create the segmentation mask.
 *
 * Members of this interface can change without a major version bump to accommodate new browser
 * bugs and capabilities. If you extend this type, you might need to rework your code for new minor
 * versions of this library.
 */
export default interface BackgroundFilterSpec extends AssetSpec {
  /**
   * Paths to resources that need to be loaded for background blur.
   */
  paths?: BackgroundFilterPaths;

  /**
   * Specification to define the parameters for ML model that does the foreground and background
   * segmentation.
   */
  model?: ModelSpec;
}

/**
 * A set of options that can be supplied when creating a background blur video frame processor.
 */
export default interface BackgroundBlurOptions {
  /** A {@link Logger|Logger} to which log output will be written. */
  logger?: Logger; 
 /** The amount of blur that will be applied to a video stream. */
 blurStrength?: number;
  /** How often the video frame processor will report observed events*/
  reportingPeriodMillis?: number;
}
```

This example shows how to create a processor with custom paths, and set options for processor assets\.

```
const blurPaths: BackgroundFilterPaths = {
  worker: `https://somepath/worker.js`,
  wasm: `https://somepath/_cwt-wasm.wasm`,
  simd: `https://somepath/_cwt-wasm-simd.wasm`,
};

const blurModel = ModelSpecBuilder.builder()
    .withDefaultModel()
    .withPath(`https://somepath/model.tflite`)
    .build();
    
    
const blurSpec: BackgroundFilterSpec = {
    paths: blurPaths,
    model: blurModel,
};

const blurOptions: BackgroundBlurOptions = {
    logger: new ConsoleLogger('MyLogger', LogLevel.INFO),
    reportingPeriodMillis: 1000,
    blurStrength: 10,
};

const processors = [];
// create blur processor
if (BackgroundBlurVideoFrameProcessor.isSupported(blurSpec, blurOptions)) {
    const blurProcessor = await BackgroundBlurVideoFrameProcessor.create(blurSpec, blurOptions);
    processors.push(blurProcessor);
}

// add the processor to the pipeline
let transformDevice = new DefaultVideoTransformDevice(logger, device, processors);
```

## Using a custom model for background segmentation<a name="segmentation-model"></a>

The Amazon Chime SDK comes with a default background segmentation machine learning \(ML\) model that you can use to separate the person in the foreground from the background\. The ML model generates a mask that applies a background blur only to the pixels that you turn on in the mask\. Through the `BackgroundFilterSpec`, you can provide your own ML model for segmenting the background from the foreground\. To override the default model, you need to create a `BackgroundFilterSpec` and define the model field\.

This example shows how to create a background filter and define a model field\.

```
/**
* An interface for defining the input and output shape of an ML model 
* along with the path to download the model.
*/
export default interface ModelSpec {
    path: string;
    input: ModelShape;
    output: ModelShape;
}

/**
 * An interface to define the shape of an ML model. 
 * Use this to define the input and output shape of an ML model.
 */
export default interface ModelShape {
    height: number;
    width: number;
    range: number[];
    channels: number;
}

const inputShape: ModelShape = {
    height: 144,
    width: 256,
    range: [0, 1],
    channels: 3,
};

const outputShape: ModelShape = {
    height: 144,
    width: 256,
    range: [0, 1],
    channels: 1,
};

const blurModel = ModelSpecBuilder.builder()
    .withPath(`https://somepath/model.tflite`)
    .withInput(inputShape)
    .withOutput(outputShape)
})
.build();
    
bbprocessor = await BackgroundBlurVideoFrameProcessor.create({
    model: blurModel
});
```

## Observing processor events<a name="processor-events"></a>

The video frame processor provides an event that indicates performance issues\. You can observer this event and receive notifications when the processor duration runs for too long\. You can then use the event to disable background blur\.

This example shows how to set the thresholds for long duration\.

```
export default interface BackgroundBlurVideoFrameProcessorObserver {
  /**
   * This event occurs when the amount of time it takes to apply the background blur is longer than expected. The
   * measurement is taken from the time the process method starts to when it returns. For example, if the video
   * is running at 15 frames per seconds and we are averaging more than 67 ms (1000 ms reporting period / 15 fps)
   * to apply background blur, then a very large portion of each frame's maximum processing time is taken up by
   * processing background blur.
   *
   * The observer will be called a maximum of once per {@link periodMillis}. In the event that the {@link avgFilterDurationMillis}
   * is larger than expected the builder can use this event as a trigger to disable the background blur.
   *
   * @param event
   * framesDropped: The expected number of frames based on frame rate, minus the actual number of frames.
   * avgFilterDurationMillis: The average amount of time that the processor took per frame in the reporting period.
   * framerate: The vide frame rate set by the SDK.
   * periodMillis: The duration of the reporting period in milliseconds.
   */
    filterFrameDurationHigh?: (event: {
        framesDropped: number;
        avgFilterDurationMillis: number;
        framerate: number;
        periodMillis: number;
    }) => void;
}
```

This example shows how to add the observer to a processor\.

```
/** 
* by default the events will be reported once a second. You can use the options
* to set how often to report the event
*/ 
const options: BackgroundBlurOptions = {
    reportingPeriodMillis: 2000
};

const processor = await BackgroundBlurVideoFrameProcessor.create(null, options);
processor.addObserver({
    filterFrameDurationHigh: (event) => {
        console.log(`background filter duration high: frames dropped - ${event.framesDropped}, avg - ${event.avgFilterDurationMillis} ms, frame rate - ${event.framerate}, period - ${event.periodMillis} ms`);
    }
})
```