---
description: >-
  Example code for the gettransaction  {disallowed} json-rpc method. Сomplete
  guide on how to use gettransaction  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# gettransaction {disallowed} - DigiByte

#### Parameters

`txid` - string, required

The transaction id

`include_watchonly` - boolean, optional, default=true for watch-only wallets, otherwise false

Whether to include watch-only addresses in balance calculation and details\[]

`verbose` - boolean, optional, default=false

Whether to include a decoded field containing the decoded transaction (equivalent to RPC decoderawtransaction)

#### Request

```java
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gettransaction",
"params": [null, null, null],
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
