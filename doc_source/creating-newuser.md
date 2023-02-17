# Creating a user<a name="creating-newuser"></a>

The following example show to create a user in Amazon WorkDocs\.

**Note**  
This is not a valid operation for a Connected AD configuration\. To create a user in the Connected AD configuration, the user must already be present in the enterprise directory\. Then, you must make a call to the [ActivateUser](https://docs.aws.amazon.com/workdocs/latest/APIReference/API_ActivateUser.html) API to activate the user in Amazon WorkDocs\.

The following example demonstrates how to create a user with a storage quota of 1 gigabyte\.

```
CreateUserRequest request = new CreateUserRequest();
    request.setGivenName("GivenName");
    request.setOrganizationId("d-12345678c4");
    // Passwords should:
    //   Be between 8 and 64 characters
    //   Contain three of the four below:
    //   A Lowercase Character
    //   An Uppercase Character
    //   A Number
    //   A Special Character
    request.setPassword("Badpa$$w0rd");
    request.setSurname("surname");
    request.setUsername("UserName");
    StorageRuleType storageRule = new StorageRuleType();
    storageRule.setStorageType(StorageType.QUOTA);
    storageRule.setStorageAllocatedInBytes(new Long(1048576l));
    request.setStorageRule(storageRule);
    CreateUserResult result = workDocsClient.createUser(request);
```

Follow these steps to obtain a Amazon WorkDocs organization ID from the AWS console:

**To get an organization ID**

1. In the [AWS Directory Service console](https://console.aws.amazon.com/directoryservicev2/) navigation pane, choose **Directories**\.

1. Note the **Directory ID** value that corresponds to your Amazon WorkDocs site\. That is the Organization ID for the site\.