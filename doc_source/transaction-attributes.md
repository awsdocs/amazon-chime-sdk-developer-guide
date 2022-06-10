# Using TransactionAttributes<a name="transaction-attributes"></a>

You use the `TransactionAttributes` data structure to store application\-specific information, such as call states or meeting IDs, and then pass that data to AWS Lambda invocations\. This structure removes the need for storing data in external databases such as Amazon DynamoDB\. 

`TransactionAttributes` are [JSON Objects](https://www.w3schools.com/js/js_json_objects.asp) that contain key/value pairs\. The objects can contain a maximum of 100 key/value pairs, and the objects have a maximum payload size of 20 KB\. The data in a `TransactionAttributes` structure persists for the life of a transaction\.

When an AWS Lambda function passes `TransactionAttributes` to a SIP media application, the application updates any stored attributes\. If you pass a `TransactionAttributes` object with an existing key set, you update the stored values\. If you pass a different key set, you replace the existing values with the values from that different key set\. Passing an empty map \( `{}` \) erases any stored values\.

**Topics**
+ [Setting TransactionAttributes](set-trans-attributes.md)
+ [Updating TransactionAttributes](update-trans-attributes.md)
+ [Clearing TransactionAttributes](clear-trans-attributes.md)
+ [Handling ACTION\_SUCCESSFUL events](attribute-trans-success.md)
+ [Invalid inputs](attribute-trans-invalid.md)