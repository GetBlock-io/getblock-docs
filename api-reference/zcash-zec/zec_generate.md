---
description: >-
  Example code for the generate  {disallowed} json-rpc method. Сomplete guide on
  how to use generate  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# generate {disallowed} - Zcash

#### Parameters

`numblocks` - numeric

How many blocks are generated immediately.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "generate",
"params": [2],
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
