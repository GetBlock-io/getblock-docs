---
description: >-
  Example code for the listtransactions  {disallowed} json-rpc method. Сomplete
  guide on how to use listtransactions  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# listtransactions {disallowed} - Zcash

#### Parameters

`account` - string

Optional.

DEPRECATED. The account name. Should be "\*".

`count` - numeric

Optional, default=10

The number of transactions to return.

`from` - numeric

Optional, default=0

The number of transactions to skip.

`includeWatchonly` - boolean

Optional, default=false

Whether to include watchonly addresses.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listtransactions",
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
