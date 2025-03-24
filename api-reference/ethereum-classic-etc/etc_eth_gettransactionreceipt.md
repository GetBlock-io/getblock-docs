---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Ethereum Classic

#### Parameters

`DATA` - string

32-byte hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0x01736b023300e297e3b3c7c13ab5db49c7c0be768d48790f46dd7baeefa7b047"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38",
        "blockNumber": "0xcdf167",
        "contractAddress": null,
        "cumulativeGasUsed": "0x1b454",
        "effectiveGasPrice": "0x4a817c800",
        "from": "0x7eb4c9d6b763324eea4852f5d40985bbf0f29832",
        "gasUsed": "0x1b454",
        "logs": [
            {
                "address": "0x3c42649799074b438889b80312ea9f62bc798aa8",
                "blockHash": "0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38",
                "blockNumber": "0xcdf167",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x0",
                "removed": false,
                "topics": [
                    "0xd54c6ec36a4a8371c702d90820200e8ed4e0cbe097458fac76a91ef5f75333f7",
                    "0x0000000000000000000000000000000000000000000000000000000000000017",
                    "0x000000000000000000000000000000000000000000000000000000000001a1a3",
                    "0x60dfb255ad11a4d8d2a4a6e264b0b41baf04737047bde86bfe5e16f272442154"
                ],
                "transactionHash": "0x01736b023300e297e3b3c7c13ab5db49c7c0be768d48790f46dd7baeefa7b047",
                "transactionIndex": "0x0"
            }
        ],
        "logsBloom": "0x00000000000000000000000000000100000000000100000000000000000000000000000000000000001000000000000000101000000000400000005000000000000100000000000000000000000000000000000000000000000000000000000000400000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000080000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000",
        "status": "0x1",
        "to": "0x3c42649799074b438889b80312ea9f62bc798aa8",
        "transactionHash": "0x01736b023300e297e3b3c7c13ab5db49c7c0be768d48790f46dd7baeefa7b047",
        "transactionIndex": "0x0"
    }
}
```
