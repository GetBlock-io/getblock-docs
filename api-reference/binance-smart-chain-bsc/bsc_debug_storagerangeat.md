---
description: >-
  Example code for the debug_storageRangeAt  {disallowed} json-rpc method.
  Сomplete guide on how to use debug_storageRangeAt  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# debug\_storageRangeAt {disallowed} - Binance Smart Chain

#### Parameters

`blockHash` - data

Block hash.

`txIndex` - integer

Transaction index from which to start.

`address` - data

Contract address.

`startKey` - hash

Start key.

`limit` - integer

Number of storage entries to return.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_storageRangeAt",
"params": ["0x6e6a058ac35224a9842a8010042ddc93dea2f073260a8d79cd3368a232c3e7ed", 3, "0x0a8156e7ee392d885d10eaa86afd0e323afdcd95", "0xc94770007dda54cF92009BFF0dE90c06F603a09f", 1],
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
