---
description: >-
  Example code for the hmyv2_getBlockReceipts json-rpc method. Сomplete guide on
  how to use hmyv2_getBlockReceipts json-rpc in GetBlock.io Web3 documentation.
---

# hmyv2\_getBlockReceipts - Harmony

#### Parameters

`DATA, 32 Bytes` - None

Block hash.

#### Request

```java
curl --location --request POST 'https://one.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "hmyv2_getBlockReceipts",
"params": ["0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
            "blockNumber": 42379341,
            "contractAddress": "0x0000000000000000000000000000000000000000",
            "cumulativeGasUsed": 233421,
            "from": "one1qku72aj98fskvge07h9j0gtfu9sdw62ucgff67",
            "gasUsed": 233421,
            "logs": [
                {
                    "address": "0xcf664087a5bb0237a0bad6742852ec6c8d69a27a",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x0000000000000000000000000000000000000000000000011422c225fc8c6be4",
                    "logIndex": "0x0",
                    "removed": false,
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x0000000000000000000000000e00af5734e64340d4d145b39da16a8c8aa20547",
                        "0x000000000000000000000000a24bb31c626d3b3fa6073f8663e9ac0ab81fd1f9"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0xcf664087a5bb0237a0bad6742852ec6c8d69a27a",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x000000000000000000000000000000000000000000000001129c6909c8950000",
                    "logIndex": "0x1",
                    "removed": false,
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x000000000000000000000000a24bb31c626d3b3fa6073f8663e9ac0ab81fd1f9",
                        "0x000000000000000000000000632a7f749f98812bf07fa00a8aa53e390046467e"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0x3f56e0c36d275367b8c502090edf38289b3dea0d",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x000000000000000000000000000000000000000000000000043c23e5bf569970",
                    "logIndex": "0x2",
                    "removed": false,
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x000000000000000000000000632a7f749f98812bf07fa00a8aa53e390046467e",
                        "0x000000000000000000000000c1726df372dec10ffe72a3afbae3b670b88d5abe"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0x632a7f749f98812bf07fa00a8aa53e390046467e",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x0000000000000000000000000000000000000000000000849bbfdba21edf37ed00000000000000000000000000000000000000000000217e2fb6d4438fe33b9b",
                    "logIndex": "0x3",
                    "removed": false,
                    "topics": [
                        "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0x632a7f749f98812bf07fa00a8aa53e390046467e",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001129c6909c8950000000000000000000000000000000000000000000000000000043c23e5bf5699700000000000000000000000000000000000000000000000000000000000000000",
                    "logIndex": "0x4",
                    "removed": false,
                    "topics": [
                        "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                        "0x000000000000000000000000a24bb31c626d3b3fa6073f8663e9ac0ab81fd1f9",
                        "0x000000000000000000000000c1726df372dec10ffe72a3afbae3b670b88d5abe"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0xeeeeeb57642040be42185f49c52f7e9b38f8eeee",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x0000000000000000000000000000000000000000000000001f9f65a0e913d8c3",
                    "logIndex": "0x5",
                    "removed": false,
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x000000000000000000000000c1726df372dec10ffe72a3afbae3b670b88d5abe",
                        "0x0000000000000000000000000e00af5734e64340d4d145b39da16a8c8aa20547"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0xc1726df372dec10ffe72a3afbae3b670b88d5abe",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x000000000000000000000000000000000000000000000003222f442a8299d49e000000000000000000000000000000000000000000000017584f25612a96ce69",
                    "logIndex": "0x6",
                    "removed": false,
                    "topics": [
                        "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0xc1726df372dec10ffe72a3afbae3b670b88d5abe",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x000000000000000000000000000000000000000000000000043c23e5bf569970000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001f9f65a0e913d8c3",
                    "logIndex": "0x7",
                    "removed": false,
                    "topics": [
                        "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                        "0x000000000000000000000000a24bb31c626d3b3fa6073f8663e9ac0ab81fd1f9",
                        "0x0000000000000000000000000e00af5734e64340d4d145b39da16a8c8aa20547"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0xcf664087a5bb0237a0bad6742852ec6c8d69a27a",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x0000000000000000000000000000000000000000000000000186591c33f76be4",
                    "logIndex": "0x8",
                    "removed": false,
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                        "0x000000000000000000000000a24bb31c626d3b3fa6073f8663e9ac0ab81fd1f9",
                        "0x000000000000000000000000c76dc0785ea1c23aa4cc7a77e94f894bdd9829fd"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0x0e00af5734e64340d4d145b39da16a8c8aa20547",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x000000000000000000000000000000000000000000001f3038a37fedf0f8d85200000000000000000000000000000000000000000000038fb8a3accf29434b43",
                    "logIndex": "0x9",
                    "removed": false,
                    "topics": [
                        "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                },
                {
                    "address": "0x0e00af5734e64340d4d145b39da16a8c8aa20547",
                    "blockHash": "0x910c18611575d42685a4d122a71c9d359a68c3f9786e1ca5a18c27819f8504ff",
                    "blockNumber": "0x286a84d",
                    "data": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001f9f65a0e913d8c30000000000000000000000000000000000000000000000011422c225fc8c6be40000000000000000000000000000000000000000000000000000000000000000",
                    "logIndex": "0xa",
                    "removed": false,
                    "topics": [
                        "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                        "0x000000000000000000000000a24bb31c626d3b3fa6073f8663e9ac0ab81fd1f9",
                        "0x000000000000000000000000a24bb31c626d3b3fa6073f8663e9ac0ab81fd1f9"
                    ],
                    "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
                    "transactionIndex": "0x0"
                }
            ],
            "logsBloom": "0x00200000010000000000000080020000000001000000100000000000020042000000010000000010000000000000080000020000000000000000000000000000000000000040008000000008000000200000400000000000000000000000000000000000000000001040000000000001000000000000000000900010000000100000000800000000000000000000000000000000000000080000004000000000000000000000000000000000000000000000080000004000080000000002000000000022000400008000000000000800000000000000001000000000000000000000000000000002080000000000000000082000000000000000000000000000",
            "root": "0x",
            "shardID": 0,
            "status": 1,
            "to": "one15f9mx8rzd5anlfs887rx86dvp2upl50eslufrq",
            "transactionHash": "0xef7ec4991840bd66569fa070ea9ab39b38a2233bbc5b3cfd272fa456e893b135",
            "transactionIndex": 0
        }
    ]
}
```
