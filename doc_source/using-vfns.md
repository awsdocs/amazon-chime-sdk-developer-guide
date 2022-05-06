# Using Amazon Voice Focus<a name="using-vfns"></a>

Amazon Voice Focus is a noise suppressor that uses machine learning to reduce unwanted background noise in your users’ microphone input\. Unlike conventional noise suppressors, Amazon Voice Focus reduces fan noise, road noise, typing, rustling paper, lawnmowers, barking dogs, and other kinds of non\-speech input, allowing your users to focus on the human voice\.

**Note**  
Amazon Voice Focus doesn't eliminate those types of noises; it reduces their sound levels\. To ensure privacy during a meeting, choose **Mute** to silence yourself or others\.

Amazon Voice Focus offers multiple complexity levels, allowing you to trade some quality in order to support a wider range of devices\. By default, the Amazon Chime SDK automatically chooses the right complexity level at runtime based on device performance\.

**When to use Amazon Voice Focus**  
Use Amazon Voice Focus when users might experience background noise and they only care about human speech\. Because it reduces almost all non\-voice sounds, Amazon Voice Focus works best in applications where a person’s voice is the most important part of an interaction\. In quiet environments, or in situations where other sounds are important, such as music lessons, Amazon Voice Focus can eliminate important audio\.

Also, Amazon Voice Focus can suppress quiet speech\. For example, if you have several participants in a room with a single laptop, Amazon Voice focus can suppress the quietest participants\.

Finally, applications that use a lot of CPU, such as games, might not leave enough resources for Amazon Voice Focus to work smoothly, especially on resource\-constrained devices\. If you think your application might be used in those scenarios, be sure to test your application with Amazon Voice Focus, and give users the ability to turn noise suppression on and off\.

**Note**  
Amazon Voice Focus is only available when the microphone provides a mono audio stream\.

**Amazon Voice Focus stages**  
Amazon Voice Focus steps through these stages when users turn it on:
+ **Configuration and estimation** – For a given builder intent and runtime environment, decide on runtime parameters\. Amazon Voice Focus checks for browser support and CPU speed to determine a complexity level for the user’s browser\.
+ **Preloading and initialization** – Model files have an upper bound of 8MB, and they're downloaded as needed from Amazon’s Content Delivery Network\. If possible, initialize Amazon Voice Focus prior to the start of a meeting or conference\.
+ **Interference in `getUserMedia`** – Amazon Voice Focus works best if no other noise suppression applies to a microphone input\. By default, Amazon Voice Focus disables a web browser’s built\-in noise suppressor for you\.
+ **Application to the chosen audio stream** – The SDK device controller applies noise suppression to the microphone input before use\.

For detailed information about implementing Amazon Voice Focus, see [ https://aws\.github\.io/amazon\-chime\-sdk\-js/modules/amazonvoice\_focus\.html ](https://aws.github.io/amazon-chime-sdk-js/modules/amazonvoice_focus.html)\.