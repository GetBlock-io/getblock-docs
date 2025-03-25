---
description: >-
  Example code for the /debug/connections/outgoing rest method. Сomplete guide
  on how to use /debug/connections/outgoing rest in GetBlock.io Web3
  documentation.
---

# /debug/connections/outgoing - NEM

#### Parameters

\-

#### Request

```java
curl --location --request GET 'https://go.getblock.io/YOUR-ACCESS-TOKEN/debug/connections/outgoing' \
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "most-recent": [
        {
            "elapsed-time": 2,
            "host": "xem44.allnodes.me",
            "id": 1392509,
            "path": "/transactions/unconfirmed",
            "start-time": 238620183
        },
        {
            "elapsed-time": 2,
            "host": "xem44.allnodes.me",
            "id": 1392508,
            "path": "/chain/score",
            "start-time": 238620183
        },
        {
            "elapsed-time": 6,
            "host": "108.61.168.86",
            "id": 1392507,
            "path": "/transactions/unconfirmed",
            "start-time": 238620179
        }
    ]
}
```
