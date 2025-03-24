---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - KuCoin Community Chain

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0xb036e4ff7e7f596615d121d3a1c923c618bd3c2d3745c512346d7d8e583f01f7"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x56436a935370b8decda78cf4a60e0668a882764af1cdec3a6d6967f944f4dace",
        "blockNumber": "0xc6f437",
        "contractAddress": null,
        "cumulativeGasUsed": "0x164e6",
        "from": "0xed228c1c87c9ecb44ea640fc8b0f4955e4296233",
        "gasUsed": "0x164e6",
        "logs": [
            {
                "address": "0x495d133b938596c9984d462f007b676bdc57ecec",
                "blockHash": "0x56436a935370b8decda78cf4a60e0668a882764af1cdec3a6d6967f944f4dace",
                "blockNumber": "0xc6f437",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000bd8",
                "logIndex": "0x0",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000d7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
                    "0x000000000000000000000000ed228c1c87c9ecb44ea640fc8b0f4955e4296233"
                ],
                "transactionHash": "0xb036e4ff7e7f596615d121d3a1c923c618bd3c2d3745c512346d7d8e583f01f7",
                "transactionIndex": "0x0",
                "transactionLogIndex": "0x0",
                "type": "mined"
            },
            {
                "address": "0xd7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
                "blockHash": "0x56436a935370b8decda78cf4a60e0668a882764af1cdec3a6d6967f944f4dace",
                "blockNumber": "0xc6f437",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000bd8",
                "logIndex": "0x1",
                "removed": false,
                "topics": [
                    "0x89ed24731df6b066e4c5186901fffdba18cd9a10f07494aff900bdee260d1304",
                    "0x000000000000000000000000ed228c1c87c9ecb44ea640fc8b0f4955e4296233"
                ],
                "transactionHash": "0xb036e4ff7e7f596615d121d3a1c923c618bd3c2d3745c512346d7d8e583f01f7",
                "transactionIndex": "0x0",
                "transactionLogIndex": "0x1",
                "type": "mined"
            }
        ],
        "logsBloom": "0x00000000208000000000002000000000000000000000000000000000004000000000000000000000000000000000000000008000000000a000000000000000000000000000000000000000080000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000100000000000000000000000000000000c0000000000000000200000000000000000000000800000000000000000000000000020000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000001000080000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0xd7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
        "transactionHash": "0xb036e4ff7e7f596615d121d3a1c923c618bd3c2d3745c512346d7d8e583f01f7",
        "transactionIndex": "0x0"
    }
}
```
