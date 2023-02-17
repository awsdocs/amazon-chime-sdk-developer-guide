# Using clientSessionId to send call context data<a name="call-context-data"></a>

When you create a Skill, you configure it to gather data for each call\. When your Skill gathers that data, you can use the `clientSessionId` to identify the data\. In turn, you can find that ID in the [user\-to\-user SIP header](https://datatracker.ietf.org/doc/html/rfc7433) in the `SIP INVITE`\. 

The process runs as follows:

1. The Alexa Skill stores the session context data in the Skill’s back\-end cloud service, which generates a unique identifier for the data\.

1. The Skill provides the identifier for the session context data in the `clientSessionId field` of the StartCommunicationSession API request\. 

1. Alexa Service encodes the identifier into the user\-to\-user SIP header of the SIP INVITE\. 

1. The SIP media application accepts the `SIP INVITE `and retrieves the identifier\. The application then looks up the corresponding session context data from the Skill’s back\-end cloud service\.

The `clientSessionId` is formatted as a key\-value pair and encoded using base16\. The `clientSessionId` is formatted using `csi` as the key and `=` as the separator\. If you decode the example in the previous section, the user\-to\-user SIP header would contain:

`csi=db792233-8df3-416c-8d80-a70038747b74`

The key\-value pair string is encoded using base16 as defined in [RFC 7433](https://datatracker.ietf.org/doc/html/rfc7433)\. The following example shows an encoded key\-value string\. The `encoding=hex` statement indicates the string is encoded using base16 as defined in the RFC\.

`6373693d64623739323233332d386466332d343136632d386438302d613730303338373437623734;encoding=hex`