---
description: >-
  Example code for the pruneblockchain  {disallowed} json-rpc method. Сomplete
  guide on how to use pruneblockchain  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# pruneblockchain {disallowed} - DigiByte

#### Parameters

`height` - numeric, required

The block height to prune up to. May be set to a discrete height, or to a UNIX epoch time to prune blocks whose block time is at least 2 hours older than the provided timestamp.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "pruneblockchain",
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
