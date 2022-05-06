# Using TransactionAttributes<a name="transaction-attributes"></a>

You use the `TransactionAttributes` data structure to store application\-specific information, such as call states or meeting IDs, and then pass that data to AWS Lambda invocations\. This structure removes the need for storing data in external databases such as Amazon DynamoDB\. 

`TransactionAttributes` are [JSON Objects](https://www.w3schools.com/js/js_json_objects.asp) that contain key/value pairs\. The objects can contain a maximum of 10 key/value pairs, and each key or value can contain 1,024 characters\. The information in a `TransactionAttributes` structure persists for the life of a transaction\.

When an AWS Lambda function passes a `TransactionAttributes` object, the corresponding SIP media application updates any stored attributes\. If you pass a `TransactionAttributes` object with an existing key set, you update the stored values\. If you pass a new key set, you replace the existing values with the new values\. Passing an empty map \( `{}` \) erases any stored values\.

**Topics**
+ [Setting TransactionAttributes](set-trans-attributes.md)
+ [Updating TransactionAttributes](update-trans-attributes.md)
+ [Clearing TransactionAttributes](clear-trans-attributes.md)
+ [Handling ACTION\_SUCCESSFUL events](attribute-trans-success.md)
+ [Invalid inputs](attribute-trans-invalid.md)