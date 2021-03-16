# Configuring the AWS SDK to invoke the APIs<a name="configure-sdk-invoke"></a>

```
        
    AWS.config.credentials = new AWS.Credentials(accessKeyId, secretAccessKey, null);
    const chime = new AWS.Chime({ region: 'us-east-1' });
    chime.endpoint = new AWS.Endpoint('https://service.chime.aws.amazon.com/console');
```