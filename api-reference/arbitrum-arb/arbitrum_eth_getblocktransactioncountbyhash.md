---
description: >-
  Example code for the eth_getBlockTransactionCountByHash json-rpc method.
  Сomplete guide on how to use eth_getBlockTransactionCountByHash json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getBlockTransactionCountByHash - Arbitrum

#### Parameters

`DATA` - string

hash of the block.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_getBlockTransactionCountByHash", "params": ["0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a"], "id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5"
}
```
