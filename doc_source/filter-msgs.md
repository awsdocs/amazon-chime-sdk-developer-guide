# Using filter rules to filter messages<a name="filter-msgs"></a>

The Amazon Chime SDK supports setting filter rules on an app instance user’s channel membership to limit which message they will receive\. Filter rules are set on the channel membership and will run against the message attributes map\. The message attribute map must be a map of string keys to string values\. Filter rules support inclusion and exclusion with exact string matching\.

**Important**  
The Amazon Chime SDK only supports escaped JSON strings as the filter rule\.

To set filter rules on the channel membership, use the [PutChannelMembershipPreferences](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_PutChannelMembershipPreferences.html) API\. You can include message attributes in a channel message as part of the [SendChannelMessage](https://docs.aws.amazon.com/chime-sdk/latest/APIReference/API_messaging-chime_SendChannelMessage.html) API call\.

## Filter rule types<a name="filter-rule-types"></a>

The Amazon Chime SDK supports the following types of filter rules: 
+ Inclusive exact string matching
+ Exclusive exact string matching
+ Multiple filter rules using AND or OR

## Filter rule limits<a name="filter-rule-limits"></a>

The Amazon Chime SDK imposes the following restrictions on filter rules:
+ We only support exact string matching\.
+ A total filter rules size of 2KB\.
+ A total message attribute size of 1KB\.
+ A maximum of five \(5\) separate constraints inside an OR filter rule\.
+ A maximum complexity of 20 for the entire filter rule\. *Complexity* is calculated as the sum of the number of keys and values in the filter rule:

  For example, this filter rule has a complexity of 4\.

  ```
  "FilterRule": "{\"type\":[{\"anything-but\": [\"Room\"]}],\"mention\":[\"Bob\"]}
  ```

  We calculate that value as follows:

  ```
  Keys = “type” and “mention” - Complexity 2
  Values = "Room" and "Bob" -   Complexity 2
  
                        Total complexity = 4
  ```

## Example channel membership preferences with filter rules<a name="example-preference-rule"></a>

The following examples show several ways to use channel membership preferences and filter rules\.

**Inclusive string matching**  
 This filter rule allows any message with the message attribute key “mention” and the value “Bob\.” 

```
{
    "Preferences": {
        "PushNotifications": {
            "FilterRule": "{\"mention\":[\"Bob\"]}",
            "AllowNotifications": "FILTERED"
        }
    }
}
```

An app instance user with the preferences above receives a channel message with the following message attributes:

```
"MessageAttributes": {
    "mention": {
        "StringValues": ["Bob", "Alice"]
    }
}
```

However, the app instance user will not receive a channel message with the following attributes:

```
"MessageAttributes": {
    "mention": {
        "StringValues": ["Tom"]
    }
}
```

**Exclusive string matching**  
 This filter rule allows any message except those containing the attribute key “type” and the value “Room”\. 

```
{
    "Preferences": {
        "PushNotifications": {
            "FilterRule": "{\"type\":[{\"anything-but\": [\"Room\"]}]}",
            "AllowNotifications": "FILTERED"
        }
    }
}
```

An app instance user with those preferences receives a channel message with the following message attributes:

```
"MessageAttributes": {
    "type": {
        "StringValues": ["Conversation"]
    }
}
```

However, the app instance user doesn't see a channel message with the following attributes:

```
"MessageAttributes": {
    "type": {
        "StringValues": ["Room"]
    }
}
```

**A multiple filter rule with AND logic**  
When you combine filter rules with AND logic, a message must meet all the filter criteria for the filter to apply\.

```
{
    "Preferences": {
        "PushNotifications": {
            "FilterRule": "{\"type\":[{\"anything-but\": [\"Room\"]}],\"mention\":[\"Bob\"]}",
            "AllowNotifications": "FILTERED"
        }
    }
}
```

An app instance user with the preferences above receives a channel message with the following message attributes:

```
"MessageAttributes": {
    "mention": {
        "StringValues": ["Bob"]
    },
    "type": {
        "StringValues": ["Conversation"]
    }
}
```

**A multiple filter rule with OR logic**  
You use `$or` to combine filter rules with OR logic\. When you use OR logic, a message must meet one of the criteria for the filter to apply\. 

```
{
    "Preferences": {
        "PushNotifications": {
            "FilterRule": "{\"$or\":[{\"mention\":[\"Bob\"]},{\"type\":[{\"anything-but\": [\"Room\"]}]}]}",
            "AllowNotifications": "FILTERED"
        }
    }
}
```

An app instance user with the preferences above receives a channel message with the following message attributes:

```
"MessageAttributes": {
    "mention": {
        "StringValues": ["Bob"]
    }
}
```

An app instance user with the preferences above receives a channel message with the following message attributes:

```
"MessageAttributes": {
    "type": {
        "StringValues": ["Conversation"]
    }
}
```