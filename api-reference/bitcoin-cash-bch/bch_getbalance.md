---
description: >-
  Example code for the getbalance  {disallowed} json-rpc method. Сomplete guide
  on how to use getbalance  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getbalance {disallowed} - Bitcoin Cash

#### Parameters

`dummy` - string, optional

Remains for backward compatibility. Must be excluded or set to “\*”.

`minconf` - numeric, optional, default=0

Only include transactions confirmed at least this many times.

`include_watchonly` - boolean, optional, default=true for watch-only wallets, otherwise false

Also include balance in watch-only addresses (see ‘importaddress’)

`avoid_reuse` - boolean, optional, default=true

(only available if avoid\_reuse wallet flag is set) Do not include balance in dirty outputs; addresses are considered dirty if they have previously been used in a transaction.

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getbalance",
"params": [null, null, null, null],
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
