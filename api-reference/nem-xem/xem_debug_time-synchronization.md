---
description: >-
  Example code for the /debug/time-synchronization rest method. Сomplete guide
  on how to use /debug/time-synchronization rest in GetBlock.io Web3
  documentation.
---

# /debug/time-synchronization - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://xem.getblock.io/debug/time-synchronization' 
 --header 'x-api-key: YOUR-API-KEY' 
 --header 'Content-Type: application/json'
```

#### Response

```java
{
    "data": [
        {
            "change": 0,
            "currentTimeOffset": -8509,
            "dateTime": "2022-10-07 10:06:24"
        },
        {
            "change": 0,
            "currentTimeOffset": -8509,
            "dateTime": "2022-10-07 13:06:34"
        },
        {
            "change": 0,
            "currentTimeOffset": -8509,
            "dateTime": "2022-10-07 16:06:41"
        }
    ]
}
```
