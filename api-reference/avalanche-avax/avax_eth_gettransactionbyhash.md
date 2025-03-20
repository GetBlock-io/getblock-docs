---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Avalanche

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getTransactionByHash",
  "params": ["0x3dee39f608a0da41254143baf1d709ff1fae84150b57b3e037d206946826161e"],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "accessList": [],
        "blockHash": "0x3e56c97d34f03b1369c351fa6c9f57c8bfa987c7da40964fab981303e0ef5849",
        "blockNumber": "0xca8de4",
        "chainId": "0xa86a",
        "from": "0x9b137fcbe71543fa5c68e681e63463f7d4e0838f",
        "gas": "0x2770a",
        "gasPrice": "0x28479a8200",
        "hash": "0x3dee39f608a0da41254143baf1d709ff1fae84150b57b3e037d206946826161e",
        "input": "0x676528d1000000000000000000000000000000000000000000000086f19262e41d0160c00000000000000000000000000000000000000000000000003527b9874b15a36800000000000000000000000000000000000000000000000000000000000000a00000000000000000000000009b137fcbe71543fa5c68e681e63463f7d4e0838f000000000000000000000000000000000000000000000000000000006253f8dd000000000000000000000000000000000000000000000000000000000000000200000000000000000000000050c72103940d419fb64448f258f7eabba784f84b000000000000000000000000b31f66aa3c1e785363f0875a1b74e27b85fd66c7",
        "maxFeePerGas": "0x28479a8200",
        "maxPriorityFeePerGas": "0x28479a8200",
        "nonce": "0x1",
        "r": "0x84f0850ba67f7c99787b24fadb7597c292335ca87fc2f4f12a0b09ee532a0f53",
        "s": "0x5e03ca4521fa6ba1b31a5022feea89c051ec98dedc0de903edc2e830b8e3c483",
        "to": "0x60ae616a2155ee3d9a68541ba4544862310933d4",
        "transactionIndex": "0x0",
        "type": "0x2",
        "v": "0x0",
        "value": "0x0"
    }
}
```
