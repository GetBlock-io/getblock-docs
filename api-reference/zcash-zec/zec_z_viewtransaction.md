---
description: >-
  Example code for the z_viewtransaction  {disallowed} json-rpc method. Сomplete
  guide on how to use z_viewtransaction  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# z\_viewtransaction {disallowed} - Zcash

#### Parameters

`txid` - string

The transaction id.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_viewtransaction",
"params": [null],
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
