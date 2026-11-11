---
description: >-
  Example code for the getrpcinfo json-rpc method. Сomplete guide on how to use
  getrpcinfo json-rpc in GetBlock.io Web3 documentation.
---

# getrpcinfo - Bitcoin Gold

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getrpcinfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "active_commands": [
            {
                "method": "getrpcinfo",
                "duration": 35
            }
        ],
        "logpath": "/coin/data/debug.log"
    }
}
```
