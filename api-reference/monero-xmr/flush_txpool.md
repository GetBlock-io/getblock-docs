---
description: >-
  Example code for the flush_txpool  {disallowed} json-rpc method. Сomplete
  guide on how to use flush_txpool  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# flush\_txpool {disallowed} - Monero

#### Parameters

`txids` - array of strings

Optional, list of transactions IDs to flush from pool (all tx ids flushed if empty).

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "flush_txpool",
"params": {"txids": "dc16fa8eaffe1484ca9014ea050e13131d3acf23b419f33bb4cc0b32b6c49308"},
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
