---
description: >-
  Example code for the debug_storageRangeAt  {disallowed} json-rpc method.
  Сomplete guide on how to use debug_storageRangeAt  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# debug\_storageRangeAt {disallowed} - Ethereum Classic

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
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_storageRangeAt",
"params": [null, null, null, null, null],
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
