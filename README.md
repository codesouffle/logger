# Intro
Provides a server, backed by MongoDB, which will maintain counters and logging information for a distributed system.

# Counters
## Configuration
```javascript
{
  "counters": {
    "collection": "counters",
    "intervals": {
       "1s": {
          "interval": "1s",
          "duration": "5m"
       },
       "30s": {
          "interval": "30s",
          "duration": "1h"
       }
    }
  }
}
```

## Storage
One document is stored in the configured counters collection.  This document can be stored in any arbitrary collection as long as no other document in the collection has and attribute `__type` set to `counters`.  The document holds embedded subdocuments each containing two attributes `last_read` and `count`.

```javascript
{
  {
    "last_read": "timestamp",
    "count":     0
  }
}
```

# Logging
## Configuration
```javascript
{
  "logging": {
    "maxSize": 400,
    "indexes": []
  }
}
```

## Storage
Log messages are stored in two seperate ways.  The first is a straight storage of the message as plain documents.  Each message is stored as added with an additional timestamp and hash of the json blob

```javascript
{
  "timestamp": "1231245151",
  "hash":      "sha1 of your json blob",

  "code":          "3454",
  "message":       "your data",
  "another_field": "more of your data"
}
```

