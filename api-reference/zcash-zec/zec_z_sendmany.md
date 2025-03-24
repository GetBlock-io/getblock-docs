---
description: >-
  Example code for the z_sendmany  {disallowed} json-rpc method. Сomplete guide
  on how to use z_sendmany  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# z\_sendmany {disallowed} - Zcash

#### Parameters

`fromaddress` - string

The transparent or shielded address to send the funds from.

`amounts` - array

An array of json objects representing the amounts to send.

`minconf` - numeric,

Optional, default=1

Only include transactions confirmed at least this many times.

`fee` - numeric

Optional, default=0.00001

The fee amount to attach to this transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_sendmany",
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
