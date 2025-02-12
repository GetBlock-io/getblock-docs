---
description: >-
  Example code for the getmemoryinfo json-rpc method. Сomplete guide on how to
  use getmemoryinfo json-rpc in GetBlock.io Web3 documentation.
---

# getmemoryinfo - Zcash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getmemoryinfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "locked": {
            "chunks_free": 3,
            "chunks_used": 112,
            "free": 61952,
            "locked": 65536,
            "total": 65536,
            "used": 3584
        }
    }
}
```
