---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Gnosis

#### Parameters

`DATA` - hex string

32-byte hash of a transaction.

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
        "blockNumber": "0x1ac46e6",
        "contractAddress": null,
        "cumulativeGasUsed": "0x17b69e",
        "effectiveGasPrice": "0xbecb6240",
        "from": "0x71e11e8b1c1a2bf01934386c959aa65c4288d11e",
        "gasUsed": "0xffbae",
        "logs": [
            {
                "address": "0x7349c9eaa538e118725a6130e0f8341509b9f8a0",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x00000000000000000000000000000000000000000000000002369e05016a7f95",
                "logIndex": "0xf",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "0x0000000000000000000000002c15338cadd34753ddeccfc22762ddd981c671a4"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0x7349c9eaa538e118725a6130e0f8341509b9f8a0",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x00000000000000000000000000000000000000000000000002369e05016a7f950000000000000000000000000000000000000000036004fa8476a2a3302a6695",
                "logIndex": "0x10",
                "removed": false,
                "topics": [
                    "0x4c209b5fc8ad50758f13e2e1088ba56a560dff690a1c6fef26394f4c03821c4f",
                    "0x0000000000000000000000002c15338cadd34753ddeccfc22762ddd981c671a4"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0x6a7ced66902d07066ad08c81179d17d0fbe36829",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x000000000000000000000000e91d153e0b41518a2ce8dd3d7944fa863463a97dfffffffffffffffffffffffffffffffffffffffffffffd6c6cf4ab875e43ffff",
                "logIndex": "0x11",
                "removed": false,
                "topics": [
                    "0xda919360433220e13b51e8c211e490d148e61a3bd53de8c097194e458b97f3e1",
                    "0x00000000000000000000000071e11e8b1c1a2bf01934386c959aa65c4288d11e",
                    "0x00000000000000000000000080dc050a8c923c0051d438026f1192d53033728c"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0x6a7ced66902d07066ad08c81179d17d0fbe36829",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x0000000000000000000000000000000000000000000000068155a43676e00000",
                "logIndex": "0x12",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000000000000000000000000000000000000000000000",
                    "0x00000000000000000000000071e11e8b1c1a2bf01934386c959aa65c4288d11e"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0x6a7ced66902d07066ad08c81179d17d0fbe36829",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x0000000000000000000000000000000000000000000000068155a43676e00000000000000000000000000000000000000000000003775c7c56d3623d61d7237d",
                "logIndex": "0x13",
                "removed": false,
                "topics": [
                    "0x2f00e3cdd69a77be7ed215ec7b2a36784dd158f921fca79ac29deffa353fe6ee",
                    "0x00000000000000000000000080dc050a8c923c0051d438026f1192d53033728c",
                    "0x00000000000000000000000071e11e8b1c1a2bf01934386c959aa65c4288d11e"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0x5b8d36de471880ee21936f328aab2383a280cb2a",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x0000000000000000000000000000000000000000002ca640c476a19025a4c05f0000000000000000000000000000000000000000005efd92ec4bf7d8de9a5e9b000000000000000000000000000000000000000000420b15243ea73a65479e3d0000000000000000000000000000000000000000036004fa8476a2a3302a6695000000000000000000000000000000000000000003775c7c56d3623d61d7237d",
                "logIndex": "0x14",
                "removed": false,
                "topics": [
                    "0x804c9b842b2748a22bb64b345453a3de7ca54a6ca45ce00d415894979e22897a",
                    "0x000000000000000000000000e91d153e0b41518a2ce8dd3d7944fa863463a97d"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0xe91d153e0b41518a2ce8dd3d7944fa863463a97d",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x0000000000000000000000000000000000000000000000068155a43676e00000",
                "logIndex": "0x15",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000007349c9eaa538e118725a6130e0f8341509b9f8a0",
                    "0x00000000000000000000000080dc050a8c923c0051d438026f1192d53033728c"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0x5b8d36de471880ee21936f328aab2383a280cb2a",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x00000000000000000000000080dc050a8c923c0051d438026f1192d53033728c0000000000000000000000000000000000000000000000068155a43676e000000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000420b15243ea73a65479e3d",
                "logIndex": "0x16",
                "removed": false,
                "topics": [
                    "0xc6a898309e823ee50bac64e45ca8adba6690e99e7841c45d754e2a38e9019d9b",
                    "0x000000000000000000000000e91d153e0b41518a2ce8dd3d7944fa863463a97d",
                    "0x00000000000000000000000071e11e8b1c1a2bf01934386c959aa65c4288d11e",
                    "0x0000000000000000000000000000000000000000000000000000000000000000"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            },
            {
                "address": "0xe91d153e0b41518a2ce8dd3d7944fa863463a97d",
                "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
                "blockNumber": "0x1ac46e6",
                "data": "0x0000000000000000000000000000000000000000000000068155a43676e00000",
                "logIndex": "0x17",
                "removed": false,
                "topics": [
                    "0x7fcf532c15f0a6db0bd6d0e038bea71d30d808c7d98cb3bf7268a95bf5081b65",
                    "0x00000000000000000000000080dc050a8c923c0051d438026f1192d53033728c"
                ],
                "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
                "transactionIndex": "0x4"
            }
        ],
        "logsBloom": "0x00000000004000000000000008000100000000000000000000042000000001000000000000080000000000000000000000010000000010000000010000010000000000000000000000000008000020000000000000400000000000000000110040000000020000000000000000000800000004008028040000000098000008000000000000000000000000100000000000000000000000000000000000000000000000000000000000010000800000000800020000000000000000000000010000000002000000000000002000020000600022000000000000000002000022000000000000000001000000000000000100000000000100000020000000040000",
        "status": "0x1",
        "to": "0x80dc050a8c923c0051d438026f1192d53033728c",
        "transactionHash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
        "transactionIndex": "0x4",
        "type": "0x2"
    }
}
```
