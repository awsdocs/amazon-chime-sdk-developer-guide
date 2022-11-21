# Web application component architecture<a name="web-app-comp-arch"></a>

This diagram shows the architecture of an Amazon Chime SDK web client application:

![\[The architecture of an Amazon Chime SDK web application.\]](http://docs.aws.amazon.com/chime-sdk/latest/dg/images/architecture-2.png)

A web application typically consists of an HTML and CSS user interface layer powered by the application business logic layer\. You can build the web application in plain HTML and JavaScript, or you can use UI frameworks such as React and Angular\.

The web application’s business logic layer interacts with the Amazon Chime SDK for JavaScript through a set of JavaScript APIs\. The [ DefaultMeetingSession ](https://aws.github.io/amazon-chime-sdk-js/classes/defaultmeetingsession.html) is the root object of the SDK\. When building a server application you use [ MeetingSessionConfiguration ](https://aws.github.io/amazon-chime-sdk-js/classes/meetingsessionconfiguration.html) to initialize it with meeting and attendee information and join the meeting\. The DefaultMeetingSession also exposes the [ AudioVideoFacade ](https://aws.github.io/amazon-chime-sdk-js/interfaces/audiovideofacade.html), which enables the business logic layer to take actions, and to register callbacks that update the user interface when the underlying state of the session changes\.

The Amazon Chime SDK SDK for JavaScript is open\-source and has a set of customizable components that you can override as needed\. The default implementations allow you to build a complete unified communications application such as our demo MeetingV2 application\. The Amazon Chime SDK for JavaScript depends on two other libraries:
+  [ Browser\-Detect ](https://www.npmjs.com/package/browser-detect) for identifying the browser type and capabilities\. 
+  [ ProtoBufJs ](https://www.npmjs.com/package/protobufjs) to encode and decode signaling commands and responses needed to join a media sessions\.

The SDK also depends on the browser or Electron application to provide the Device Management APIs and the WebRTC implementation for an audio\-video session\.

The source Amazon Chime SDK for JavaScript is in TypeScript, but you can use the TypeScript comiler to compile it to JavaScript\. You can then bundle it using a module bundler such as Webpack\. As a best practice, install the Amazon Chime SDK for JavaScript from the NPM registry, and then use it in a CommonJS environment\. AWS also provides a rollup script for bundling the Amazon Chime SDK into a minified JS file in case you want to directly include it as a [ script tag in your HTML ](http://amazonaws.com/https://github.com/aws/amazon-chime-sdk-js/tree/master/demos/singlejs)\. 