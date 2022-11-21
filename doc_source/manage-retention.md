# Managing message retention<a name="manage-retention"></a>

Account owners can use the Amazon Chime SDK APIs to turn on retention for messaging\. Messages are automatically deleted based on the time period set by the administrator\. Retention periods can last from one day to 15 years\. You can also use the APIs to update message retention periods or turn off message retention at any time\.

**Topics**
+ [Example CLI retention commands](#retention-examples)
+ [Enabling message retention](#enable-retention)
+ [Restoring and deleting messages](#restore-and-delete)

## Example CLI retention commands<a name="retention-examples"></a>

The following examples show typical CLI commands for retention:

**Enabling**

`aws chime-sdk-identity put-app-instance-retention-settings --app-instance-arn {appInstanceArn} --app-instance-retention-settings ChannelRetentionSettings={RetentionDays=60}`

**Updating**

`aws chime-sdk-identity put-app-instance-retention-settings --app-instance-arn {appInstanceArn} --app-instance-retention-settings ChannelRetentionSettings={RetentionDays=30}`

**Disabling**

`aws chime-sdk-identity put-app-instance-retention-settings --app-instance-arn {appInstanceArn} --app-instance-retention-settings ChannelRetentionSettings={}`

## Enabling message retention<a name="enable-retention"></a>

You use the Amazon Chime SDK APIs to turn on retention for messaging\. You can also use the APIs to update message retention periods or turn off message retention at any time\. For more information about configuring messaging retention, see the [Amazon Chime SDK API Reference](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/Welcome.html)\.

## Restoring and deleting messages<a name="restore-and-delete"></a>

You can restore messages to users within 30 days of setting or updating a message\-retention period\. However, after that 30\-day grace period, all messages that fall under the retention period are permanently deleted, and new messages are permanently deleted as soon as they pass the retention period\.

**Note**  
During the 30\-day grace period, if you the extend the retention policy, or you turn it off, messages that haven't passed the new retention period become visible again to the users in the account\.

Messages are also permanently deleted when an `AppInstanceUser` deletes a channel or a message\.