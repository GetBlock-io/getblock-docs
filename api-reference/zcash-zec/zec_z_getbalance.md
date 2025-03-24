---
description: >-
  Example code for the z_getbalance  {disallowed} json-rpc method. Сomplete
  guide on how to use z_getbalance  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# z\_getbalance {disallowed} - Zcash

#### Parameters

`address` - string

The selected address. It may be a transparent or private address.

`minconf` - numeric

Optional, default=1

Only include transactions confirmed at least this many times.

`inZat` - boolean

Optional, default=false

Get the result amount in zatoshis (as an integer).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_getbalance",
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
