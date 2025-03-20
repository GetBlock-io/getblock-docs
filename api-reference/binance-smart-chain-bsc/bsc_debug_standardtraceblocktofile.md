---
description: >-
  Example code for the debug_standardTraceBlockToFile  {disallowed} json-rpc
  method. Сomplete guide on how to use debug_standardTraceBlockToFile 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# debug\_standardTraceBlockToFile {disallowed} - Binance Smart Chain

#### Parameters

`blockHash` - data

Block hash.

`txHash` - data

The transaction hash. Optional. If omitted, then a trace file is generated for each transaction in the block.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "debug_standardTraceBlockToFile",
"params": ["0x0cf46846c9f2abef8e40ed2f8deea4b789464f44284efe25d443e8d272393fce", null],
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
