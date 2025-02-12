---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Fuse Network

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://fuse.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0x2a0a813a6e9be82009ff572656cf8bc04fad290405aa70fd18fd101b303911fd"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x21056b00f69c1d8f9e0caf94914efe3119cb1668163a9e432c01a9f900e37249",
        "blockNumber": "0xc63461",
        "contractAddress": null,
        "cumulativeGasUsed": "0x164e6",
        "effectiveGasPrice": "0x3b9aca00",
        "from": "0xee1f8da0139fba1ead54ea3e50033e075237aa6f",
        "gasUsed": "0x164e6",
        "logs": [
            {
                "address": "0x495d133b938596c9984d462f007b676bdc57ecec",
                "blockHash": "0x21056b00f69c1d8f9e0caf94914efe3119cb1668163a9e432c01a9f900e37249",
                "blockNumber": "0xc63461",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000aa0",
                "logIndex": "0x0",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000d7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
                    "0x000000000000000000000000ee1f8da0139fba1ead54ea3e50033e075237aa6f"
                ],
                "transactionHash": "0x2a0a813a6e9be82009ff572656cf8bc04fad290405aa70fd18fd101b303911fd",
                "transactionIndex": "0x0",
                "transactionLogIndex": "0x0",
                "type": "mined"
            },
            {
                "address": "0xd7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
                "blockHash": "0x21056b00f69c1d8f9e0caf94914efe3119cb1668163a9e432c01a9f900e37249",
                "blockNumber": "0xc63461",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000aa0",
                "logIndex": "0x1",
                "removed": false,
                "topics": [
                    "0x89ed24731df6b066e4c5186901fffdba18cd9a10f07494aff900bdee260d1304",
                    "0x000000000000000000000000ee1f8da0139fba1ead54ea3e50033e075237aa6f"
                ],
                "transactionHash": "0x2a0a813a6e9be82009ff572656cf8bc04fad290405aa70fd18fd101b303911fd",
                "transactionIndex": "0x0",
                "transactionLogIndex": "0x1",
                "type": "mined"
            }
        ],
        "logsBloom": "0x00000000200000000000002000000000000000000000000000000000004000000000000000000000000000000000000000008000000000a000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000100000000000000000000000000008000c0000000000000000200000000000000000000000800000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000001000080000000000000000000000000000000000000",
        "status": "0x1",
        "to": "0xd7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
        "transactionHash": "0x2a0a813a6e9be82009ff572656cf8bc04fad290405aa70fd18fd101b303911fd",
        "transactionIndex": "0x0",
        "type": "0x0"
    }
}
```
