---
description: >-
  Example code for the z_gettotalbalance  {disallowed} json-rpc method. Сomplete
  guide on how to use z_gettotalbalance  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# z\_gettotalbalance {disallowed} - Zcash

#### Parameters

`minconf` - numeric

Optional, default=1

Only include private and transparent transactions confirmed at least this many times.

`includeWatchonly` - boolean

Optional, default=false

Also include balance in watchonly addresses.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "z_gettotalbalance",
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
