---
description: >-
  Example code for the gettransaction  {disallowed} json-rpc method. Сomplete
  guide on how to use gettransaction  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# gettransaction {disallowed} - Zcash

#### Parameters

`txid` - string

The transaction id.

`includeWatchonly` - boolean

Optional, default=false

Whether to include watchonly addresses in balance calculation and details\[]

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gettransaction",
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
