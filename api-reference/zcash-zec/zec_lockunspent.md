---
description: >-
  Example code for the lockunspent  {disallowed} json-rpc method. Сomplete guide
  on how to use lockunspent  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# lockunspent {disallowed} - Zcash

#### Parameters

`unlock` - boolean

Whether to unlock (true) or lock (false) the specified transactions

`transactions` - string

A json array of objects. Each object has txid (string) and vout (numeric).

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "lockunspent",
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
