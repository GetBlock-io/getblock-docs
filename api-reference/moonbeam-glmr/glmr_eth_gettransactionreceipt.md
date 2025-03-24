---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Moonbeam

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
        "blockNumber": "0x3752e7",
        "contractAddress": null,
        "cumulativeGasUsed": "0x8c08d",
        "effectiveGasPrice": "0x1dcdfee882",
        "from": "0x7e175bd3c50e3a9fd58c468233a36bcb97690bbf",
        "gasUsed": "0x4c3cc",
        "logs": [
            {
                "address": "0xacc15dc74880c9944775448304b263d191c6077f",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000000000062b812107c5077d",
                "logIndex": "0xb",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000e7fa4105cc4c3e566922da4dbd47a3ae5171b4de",
                    "0x000000000000000000000000a927e1e1e044ca1d9fe1854585003477331fe2af"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x0"
            },
            {
                "address": "0xffffffff1fcacbd218edc0eba20fc2308c778080",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000000000000000000e05914b",
                "logIndex": "0xc",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000a927e1e1e044ca1d9fe1854585003477331fe2af",
                    "0x000000000000000000000000e4b52aed43938894d9d73656758f25ea91abdfda"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x1"
            },
            {
                "address": "0xa927e1e1e044ca1d9fe1854585003477331fe2af",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000004cd4e00d06724d664f430000000000000000000000000000000000000000000000000000af0b4864afdf",
                "logIndex": "0xd",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x2"
            },
            {
                "address": "0xa927e1e1e044ca1d9fe1854585003477331fe2af",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000000000062b812107c5077d00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000e05914b",
                "logIndex": "0xe",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000e7fa4105cc4c3e566922da4dbd47a3ae5171b4de",
                    "0x000000000000000000000000e4b52aed43938894d9d73656758f25ea91abdfda"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x3"
            },
            {
                "address": "0xa423e7eeb60547d9c7b65005477b63ae7ce67e62",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x00000000000000000000000000000000000000000000003f594ac4df67f5006c",
                "logIndex": "0xf",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000e4b52aed43938894d9d73656758f25ea91abdfda",
                    "0x00000000000000000000000030172290d10db83bc84e6ab5ed60d057495e86b0"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x4"
            },
            {
                "address": "0xe4b52aed43938894d9d73656758f25ea91abdfda",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000005f981d21470acbb9cb0000000000000000000000000000000000000000000000000000000015267a4f4b",
                "logIndex": "0x10",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x5"
            },
            {
                "address": "0xe4b52aed43938894d9d73656758f25ea91abdfda",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000e05914b00000000000000000000000000000000000000000000003f594ac4df67f5006c0000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x11",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000e7fa4105cc4c3e566922da4dbd47a3ae5171b4de",
                    "0x00000000000000000000000030172290d10db83bc84e6ab5ed60d057495e86b0"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x6"
            },
            {
                "address": "0x818ec0a7fe18ff94269904fced6ae3dae6d6dc0b",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000000000000000000001f110",
                "logIndex": "0x12",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x00000000000000000000000030172290d10db83bc84e6ab5ed60d057495e86b0",
                    "0x000000000000000000000000b929914b89584b4081c7966ac6287636f7efd053"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x7"
            },
            {
                "address": "0x30172290d10db83bc84e6ab5ed60d057495e86b0",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x0000000000000000000000000000000000000000000000000000000003d7dfa1000000000000000000000000000000000000000000007d431fdddad4d17c8c95",
                "logIndex": "0x13",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x8"
            },
            {
                "address": "0x30172290d10db83bc84e6ab5ed60d057495e86b0",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003f594ac4df67f5006c000000000000000000000000000000000000000000000000000000000001f1100000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x14",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000e7fa4105cc4c3e566922da4dbd47a3ae5171b4de",
                    "0x000000000000000000000000b929914b89584b4081c7966ac6287636f7efd053"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0x9"
            },
            {
                "address": "0xacc15dc74880c9944775448304b263d191c6077f",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x0000000000000000000000000000000000000000000000000632b8ac495de702",
                "logIndex": "0x15",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000b929914b89584b4081c7966ac6287636f7efd053",
                    "0x000000000000000000000000e7fa4105cc4c3e566922da4dbd47a3ae5171b4de"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0xa"
            },
            {
                "address": "0xb929914b89584b4081c7966ac6287636f7efd053",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x0000000000000000000000000000000000000000000000000000000ecbf9a638000000000000000000000000000000000000000000002f6052cfad24bd37c084",
                "logIndex": "0x16",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0xb"
            },
            {
                "address": "0xb929914b89584b4081c7966ac6287636f7efd053",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x000000000000000000000000000000000000000000000000000000000001f110000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000632b8ac495de702",
                "logIndex": "0x17",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000e7fa4105cc4c3e566922da4dbd47a3ae5171b4de",
                    "0x000000000000000000000000e7fa4105cc4c3e566922da4dbd47a3ae5171b4de"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0xc"
            },
            {
                "address": "0xe7fa4105cc4c3e566922da4dbd47a3ae5171b4de",
                "blockHash": "0xd630e02dd2ee043d4ca55399582db9a5313a2cb6a1c66c949ea470a9fe5673e3",
                "blockNumber": "0x3752e7",
                "data": "0x0000000000000000000000000000000000000000000000000007378b4198df8500000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000001",
                "logIndex": "0x18",
                "removed": false,
                "topics": [
                    "0xb56c0a6e93b3daac0e43a5c2312faac3c2e39ce4fe753e8374734d743e603495"
                ],
                "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
                "transactionIndex": "0x1",
                "transactionLogIndex": "0xd"
            }
        ],
        "logsBloom": "0x00a00000800020000000000080000000040000000000000000000004000008000000000000002000000100000000000800000000000800000400000000000001000000000000000000800008404000200000000400001000000000104000000000000000000000000000000000200000000400010000000100000010000000000000400000000000000000000000004000000000000001080000004000000000000000000000008001000000000100400000040000000400000000000010000000000002000020000000000000000000004108000009101000000008000000020000000000000010000000000000000000000000000000000004800000000000",
        "status": "0x1",
        "to": "0xe7fa4105cc4c3e566922da4dbd47a3ae5171b4de",
        "transactionHash": "0x4ab47371392d03d9abd8cdbd0997b711b64ac57df0ae28717328ed56621b2c50",
        "transactionIndex": "0x1",
        "type": "0x2"
    }
}
```
