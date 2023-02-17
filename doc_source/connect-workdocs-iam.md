# Connect to Amazon WorkDocs with IAM user credentials and query for users<a name="connect-workdocs-iam"></a>

The following code shows how to use an IAM user's API credentials to make API calls\. In this case the API user and the Amazon WorkDocs site belong to the same AWS account\.

**Note**  
For greater security, create federated users instead of IAM users whenever possible\.

Ensure that the IAM user has been granted Amazon WorkDocs API access through an appropriate IAM policy\.

The code sample uses the [DescribeUsers](https://docs.aws.amazon.com/workdocs/latest/APIReference/API_DescribeUsers.html) API to search for users, and obtain metadata for users\. User metadata provides details such as first name, last name, user ID and root Folder ID\. Root folder ID is particularly helpful if you want to perform any content upload or download operations on behalf of the user\.

The code requires that you obtain an Amazon WorkDocs Organization ID\.

Follow these steps to obtain a Amazon WorkDocs organization ID from the AWS console:

**To get an organization ID**

1. In the [AWS Directory Service console](https://console.aws.amazon.com/directoryservicev2/) navigation pane, choose **Directories**\.

1. Note the **Directory ID** value that corresponds to your Amazon WorkDocs site\. That is the Organization ID for the site\.

The following example shows how to use IAM credentials to make API calls\.

```
import java.util.ArrayList;
import java.util.List;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.workdocs.AmazonWorkDocs;
import com.amazonaws.services.workdocs.AmazonWorkDocsClient;
import com.amazonaws.services.workdocs.model.DescribeUsersRequest;
import com.amazonaws.services.workdocs.model.DescribeUsersResult;
import com.amazonaws.services.workdocs.model.User;

public class GetUserDemo {

  public static void main(String[] args) throws Exception {
    AWSCredentials longTermCredentials =
        new BasicAWSCredentials("accessKey", "secretKey");
    AWSStaticCredentialsProvider staticCredentialProvider =
        new AWSStaticCredentialsProvider(longTermCredentials);

    AmazonWorkDocs workDocs =
        AmazonWorkDocsClient.builder().withCredentials(staticCredentialProvider)
            .withRegion(Regions.US_WEST_2).build();

    List<User> wdUsers = new ArrayList<>();
    DescribeUsersRequest request = new DescribeUsersRequest();
	
    // The OrganizationId used here is an example and it should be replaced 
	   // with the OrganizationId of your WorkDocs site.
    request.setOrganizationId("d-123456789c");
    request.setQuery("joe");
	
    String marker = null;
    do {
      request.setMarker(marker);
      DescribeUsersResult result = workDocs.describeUsers(request);
      wdUsers.addAll(result.getUsers());
      marker = result.getMarker();
    } while (marker != null);
	
    System.out.println("List of users matching the query string: joe ");
    
	for (User wdUser : wdUsers) {
      System.out.printf("Firstname:%s | Lastname:%s | Email:%s | root-folder-id:%s\n",
          wdUser.getGivenName(), wdUser.getSurname(), wdUser.getEmailAddress(),
          wdUser.getRootFolderId());
    }
  }
}
```