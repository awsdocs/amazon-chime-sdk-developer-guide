# Configuring the AWS SDK to invoke the APIs<a name="invoke-apis"></a>

This code sample shows you how to pass credentials to the AWS SDK, and set a region and endpoint\. 

```
    AWS.config.credentials = new AWS.Credentials(accessKeyId, secretAccessKey, null);
    const chime = new AWS.Chime({ region: 'us-east-1' });
    chime.endpoint = new AWS.Endpoint('https://service.chime.aws.amazon.com/console');
```