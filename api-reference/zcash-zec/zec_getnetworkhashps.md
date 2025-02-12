---
description: >-
  Example code for the getnetworkhashps  {disallowed} json-rpc method. Сomplete
  guide on how to use getnetworkhashps  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getnetworkhashps {disallowed} - Zcash

#### Parameters

`blocks` - numeric

Optional, default=120

The number of blocks, or -1 for blocks over difficulty averaging window.

`height` - numeric

Optional, default=-1

To estimate at the time of the given height.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getnetworkhashps",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
