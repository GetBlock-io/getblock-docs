---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - Arbitrum

#### Parameters

`DATA` - string

Hash of a block.

`QUANTITY` - integer

Transaction index position.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_getTransactionByBlockHashAndIndex", "params": ["0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a", "0x0"], "id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        "blockNumber": "0x5206d53",
        "chainId": "0xa4b1",
        "from": "0x00000000000000000000000000000000000a4b05",
        "gas": "0x0",
        "gasPrice": "0x0",
        "hash": "0xaf256a9ce8839489dcaa4f9a2740bcd5865b3db34a15434cce29e6bdcf043bc7",
        "input": "0x6bf6a42d0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000105e0460000000000000000000000000000000000000000000000000000000005206d530000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0",
        "r": "0x0",
        "s": "0x0",
        "to": "0x00000000000000000000000000000000000a4b05",
        "transactionIndex": "0x0",
        "type": "0x6a",
        "v": "0x0",
        "value": "0x0"
    }
}
```
