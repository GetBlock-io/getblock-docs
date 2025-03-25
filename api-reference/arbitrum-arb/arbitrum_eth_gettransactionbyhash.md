---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Arbitrum

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_getTransactionByHash", "params": ["0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181"], "id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xd8fe755134d44967f0c919084d3e0ec9b875c476c924f37bff17e5ef37d10d47",
        "blockNumber": "0x5911313",
        "chainId": "0xa4b1",
        "from": "0xe911f7f98ac57cf0c1cc71519d3ba720089381c4",
        "gas": "0x1a02b0",
        "gasPrice": "0x1dcd6500",
        "hash": "0xfd11ef35c7179439723e026cb7857ea5a2e48a19257bec00b0ed26672f632181",
        "input": "0x1801fbe5d5843b1d8c7345812c7ba7257e927b0c5e000000a6ca46b71aa1cdb6545660ad000000004f62e6aa5433274e333035ca261c12397df9febee0782f42749b8e0b",
        "nonce": "0x3c",
        "r": "0x87ac24f33be39a0bd27fe84a8934aa957610872606e5dbc46d296843a57930df",
        "s": "0x6ad9aba71382c20a528f3310c369b095e15991b352aea3261e2683c2b1288964",
        "to": "0xae56c981f9bb8b07e380b209fcd1498c5876fd4c",
        "transactionIndex": "0x1",
        "type": "0x0",
        "v": "0x14986",
        "value": "0x0"
    }
}
```
