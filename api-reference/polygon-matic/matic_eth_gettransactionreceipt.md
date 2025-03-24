---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Polygon

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
        "blockNumber": "0x1236eec",
        "contractAddress": null,
        "cumulativeGasUsed": "0x2a9ca",
        "effectiveGasPrice": "0x4cd5886400",
        "from": "0x9d945d909ca91937d19563e30bb4dac12c860189",
        "gasUsed": "0x2a9ca",
        "logs": [
            {
                "address": "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x0000000000000000000000000000000000000000000000000000000049040440",
                "logIndex": "0x0",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000009d945d909ca91937d19563e30bb4dac12c860189",
                    "0x000000000000000000000000c2755915a85c6f6c1c0f3a86ac8c058f11caa9c9"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xc2132d05d31c914a87c6611c10748aeb04b58e8f",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0xfffffffffffffffffffffffffffffffffffffffffffffffffffff21403659da2",
                "logIndex": "0x1",
                "removed": false,
                "topics": [
                    "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                    "0x0000000000000000000000009d945d909ca91937d19563e30bb4dac12c860189",
                    "0x0000000000000000000000001b02da8cb0d097eb8d57a175b88c7d8b47997506"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x7ceb23fd6bc0add59e62ac25578270cff1b9f619",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x000000000000000000000000000000000000000000000000050aeac778b0a5e4",
                "logIndex": "0x2",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000c2755915a85c6f6c1c0f3a86ac8c058f11caa9c9",
                    "0x0000000000000000000000002813d43463c374a680f235c428fb1d7f08de0b69"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xc2755915a85c6f6c1c0f3a86ac8c058f11caa9c9",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x000000000000000000000000000000000000000000000104001018376945bb9600000000000000000000000000000000000000000000000000000ea9b38d198a",
                "logIndex": "0x3",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xc2755915a85c6f6c1c0f3a86ac8c058f11caa9c9",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000049040440000000000000000000000000000000000000000000000000050aeac778b0a5e40000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x4",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x0000000000000000000000001b02da8cb0d097eb8d57a175b88c7d8b47997506",
                    "0x0000000000000000000000002813d43463c374a680f235c428fb1d7f08de0b69"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0xd6df932a45c0f255f85145f286ea0b292b21c90b",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x0000000000000000000000000000000000000000000000002f826bc7b58525bf",
                "logIndex": "0x5",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x0000000000000000000000002813d43463c374a680f235c428fb1d7f08de0b69",
                    "0x0000000000000000000000009d945d909ca91937d19563e30bb4dac12c860189"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x2813d43463c374a680f235c428fb1d7f08de0b69",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x0000000000000000000000000000000000000000000000ea3c80c273ee3102fc0000000000000000000000000000000000000000000008a551ffaf820ff1b9c5",
                "logIndex": "0x6",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x2813d43463c374a680f235c428fb1d7f08de0b69",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x000000000000000000000000000000000000000000000000050aeac778b0a5e4000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002f826bc7b58525bf",
                "logIndex": "0x7",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x0000000000000000000000001b02da8cb0d097eb8d57a175b88c7d8b47997506",
                    "0x0000000000000000000000009d945d909ca91937d19563e30bb4dac12c860189"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            },
            {
                "address": "0x0000000000000000000000000000000000001010",
                "blockHash": "0xf09e72e3b377a93a304a1654b3e33cc65de202c478c6468dda7eae97bcd0769a",
                "blockNumber": "0x1236eec",
                "data": "0x00000000000000000000000000000000000000000000000000cca0a84fa2e800000000000000000000000000000000000000000000003ec1393e273bfd15c21f00000000000000000000000000000000000000000000115ac9dba54e5cc78dd5000000000000000000000000000000000000000000003ec138718693ad72da1f00000000000000000000000000000000000000000000115acaa845f6ac6a75d5",
                "logIndex": "0x8",
                "removed": false,
                "topics": [
                    "0x4dfe1bbbcf077ddc3e01291eea2d5c70c2b422b415d95645b9adcfd678cb1d63",
                    "0x0000000000000000000000000000000000000000000000000000000000001010",
                    "0x0000000000000000000000009d945d909ca91937d19563e30bb4dac12c860189",
                    "0x000000000000000000000000e7e2cb8c81c10ff191a73fe266788c9ce62ec754"
                ],
                "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
                "transactionIndex": "0x0"
            }
        ],
        "logsBloom": "0x00200000000000000000000080000000000000000000000020000000000800000000000000000000020000000000000100808000000040000000040000200000000000000000000000000008000000a80800000000000000000100000000000000000001020000000000000000000000000400000000000080000010000000000000020000080000000000000000000000018000000000080200004000000000220100000000000000000000000000000000000000000010000000000000804020000002000000000081000000000000402000000000001000100000000002000010000000000000000000080000000080000000000000000004000000100002",
        "status": "0x1",
        "to": "0x1b02da8cb0d097eb8d57a175b88c7d8b47997506",
        "transactionHash": "0x6d755989f51032147484162c4dc3d6550552dbd8d3b094fe3c221bfa3c5942b2",
        "transactionIndex": "0x0",
        "type": "0x0"
    }
}
```
