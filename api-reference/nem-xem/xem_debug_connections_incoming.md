---
description: >-
  Example code for the /debug/connections/incoming rest method. Сomplete guide
  on how to use /debug/connections/incoming rest in GetBlock.io Web3
  documentation.
---

# /debug/connections/incoming - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/debug/connections/incoming' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "most-recent": [
        {
            "elapsed-time": 0,
            "host": "172.20.0.1",
            "id": 79,
            "path": "/debug/connections/incoming",
            "start-time": 238620185
        },
        {
            "elapsed-time": 0,
            "host": "172.20.0.1",
            "id": 78,
            "path": "/debug/time-synchronization",
            "start-time": 238620185
        },
        {
            "elapsed-time": 189098,
            "host": "172.20.0.1",
            "id": 77,
            "path": "/account/transfers/incoming",
            "start-time": 238431087
        }
    ]
}
```
