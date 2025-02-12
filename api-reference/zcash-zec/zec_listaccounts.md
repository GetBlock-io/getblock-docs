---
description: >-
  Example code for the listaccounts  {disallowed} json-rpc method. Сomplete
  guide on how to use listaccounts  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listaccounts {disallowed} - Zcash

#### Parameters

`minconf` - numeric

Optional, default=1

Only include transactions with at least this many.

`includeWatchonly` - boolean

Optional, default=false

Include balances in watchonly addresses.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "listaccounts",
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
