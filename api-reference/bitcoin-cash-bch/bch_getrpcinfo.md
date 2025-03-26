---
description: >-
  Example code for the getrpcinfo json-rpc method. Сomplete guide on how to use
  getrpcinfo json-rpc in GetBlock.io Web3 documentation.
---

# getrpcinfo - Bitcoin Cash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
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
                "duration": 72,
                "method": "getrpcinfo"
            }
        ],
        "logpath": "/home/bitcoin/.bitcoin/debug.log"
    }
}
```
