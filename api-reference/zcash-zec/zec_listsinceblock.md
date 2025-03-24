---
description: >-
  Example code for the listsinceblock  {disallowed} json-rpc method. Сomplete
  guide on how to use listsinceblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listsinceblock {disallowed} - Zcash

#### Parameters

`blockhash` - string

Optional

The block hash to list transactions since.

`target-confirmations` - numeric

Optional

The confirmations required, must be 1 or more.

`includeWatchonly` - boolean

Optional, default=false

Whether to include watchonly addresses.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "listsinceblock",
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
