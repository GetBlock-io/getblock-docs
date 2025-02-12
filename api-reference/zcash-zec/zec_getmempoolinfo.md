---
description: >-
  Example code for the getmempoolinfo json-rpc method. Сomplete guide on how to
  use getmempoolinfo json-rpc in GetBlock.io Web3 documentation.
---

# getmempoolinfo - Zcash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getmempoolinfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bytes": 22506,
        "size": 18,
        "usage": 54000
    }
}
```
