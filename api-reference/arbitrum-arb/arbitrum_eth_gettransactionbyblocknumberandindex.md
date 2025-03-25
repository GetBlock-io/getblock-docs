---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Arbitrum

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

`QUANTITY` - integer

Transaction index position.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_getTransactionByBlockNumberAndIndex", "params": ["latest", "0x0"], "id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x187ccf13bc440afc7fb146abf0642d4d6c02c438ea067f49f133566eef8627eb",
        "blockNumber": "0x5bc407a",
        "chainId": "0xa4b1",
        "from": "0x00000000000000000000000000000000000a4b05",
        "gas": "0x0",
        "gasPrice": "0x0",
        "hash": "0x7144169eeaff9b4b9d0a1948fa6149784710e3f55da837f93201fcfb0a6c2450",
        "input": "0x6bf6a42d000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010924a40000000000000000000000000000000000000000000000000000000005bc407a0000000000000000000000000000000000000000000000000000000000000001",
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
