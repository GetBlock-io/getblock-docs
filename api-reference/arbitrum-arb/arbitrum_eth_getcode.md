---
description: >-
  Example code for the eth_getCode json-rpc method. Сomplete guide on how to use
  eth_getCode json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getCode - Arbitrum

#### Parameters

`DATA` - string

address.

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_getCode", "params": ["0x9b956e3d318625be2686ae7268d81777c462d41f", "latest"], "id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x"
}
```
