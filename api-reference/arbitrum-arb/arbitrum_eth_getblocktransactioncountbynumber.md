---
description: >-
  Example code for the eth_getBlockTransactionCountByNumber json-rpc method.
  Сomplete guide on how to use eth_getBlockTransactionCountByNumber json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - Arbitrum

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_getBlockTransactionCountByNumber", "params": ["latest"], "id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5"
}
```
