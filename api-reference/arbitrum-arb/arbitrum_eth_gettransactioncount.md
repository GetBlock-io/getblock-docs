---
description: >-
  Example code for the eth_getTransactionCount json-rpc method. Сomplete guide
  on how to use eth_getTransactionCount json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionCount - Arbitrum

#### Parameters

`DATA` - string

address.

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_getTransactionCount", "params": ["0xb8b2522480f850eb198ada5c3f31ac528538d2f5", "latest"], "id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x48"
}
```
