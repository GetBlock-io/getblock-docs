---
description: >-
  Example code for the eth_getTransactionReceipt json-rpc method. Сomplete guide
  on how to use eth_getTransactionReceipt json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionReceipt - Cronos

#### Parameters

`DATA` - hex string

32-byte hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionReceipt",
"params": ["0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
        "blockNumber": "0x8252ec",
        "contractAddress": null,
        "cumulativeGasUsed": "0xf8973",
        "from": "0x227f6757289a86c13eee2e91c2e6eb03f2ed11a6",
        "gasUsed": "0x7a120",
        "logs": [
            {
                "address": "0xdd73dea10abc2bff99c60882ec5b2b81bb1dc5b2",
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "data": "0x00000000000000000000000000000000000000000367d92e7d6fba5cde000000",
                "logIndex": "0xf",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000227f6757289a86c13eee2e91c2e6eb03f2ed11a6",
                    "0x000000000000000000000000a922530960a1f94828a7e132ec1ba95717ed1eab"
                ],
                "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "transactionIndex": "0x2"
            },
            {
                "address": "0x2d03bece6747adc00e1a131bba1469c15fd11e03",
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "data": "0x0000000000000000000000000000000000000000002de79bcc64c83a8ffcad69",
                "logIndex": "0x10",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000a922530960a1f94828a7e132ec1ba95717ed1eab",
                    "0x000000000000000000000000814920d1b8007207db6cb5a2dd92bf0b082bdba1"
                ],
                "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "transactionIndex": "0x2"
            },
            {
                "address": "0xa922530960a1f94828a7e132ec1ba95717ed1eab",
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "data": "0x00000000000000000000000000000000000000024e3e7c36c5fea64467f3554e000000000000000000000000000000000000002bac060ac1009529892639b799",
                "logIndex": "0x11",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "transactionIndex": "0x2"
            },
            {
                "address": "0xa922530960a1f94828a7e132ec1ba95717ed1eab",
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "data": "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000367d92e7d6fba5cde0000000000000000000000000000000000000000000000002de79bcc64c83a8ffcad690000000000000000000000000000000000000000000000000000000000000000",
                "logIndex": "0x12",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000145863eb42cf62847a6ca784e6416c1682b1b2ae",
                    "0x000000000000000000000000814920d1b8007207db6cb5a2dd92bf0b082bdba1"
                ],
                "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "transactionIndex": "0x2"
            },
            {
                "address": "0xc21223249ca28397b4b6541dffaecc539bff0c59",
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "data": "0x000000000000000000000000000000000000000000000000000000000a19f269",
                "logIndex": "0x13",
                "removed": false,
                "topics": [
                    "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                    "0x000000000000000000000000814920d1b8007207db6cb5a2dd92bf0b082bdba1",
                    "0x000000000000000000000000227f6757289a86c13eee2e91c2e6eb03f2ed11a6"
                ],
                "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "transactionIndex": "0x2"
            },
            {
                "address": "0x814920d1b8007207db6cb5a2dd92bf0b082bdba1",
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "data": "0x0000000000000000000000000000000000000002eba89b3de933269e3fe10c4f000000000000000000000000000000000000000000000000000000a4fa64df5f",
                "logIndex": "0x14",
                "removed": false,
                "topics": [
                    "0x1c411e9a96e071241c2f21f7726b17ae89e3cab4c78be50e062b03a9fffbbad1"
                ],
                "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "transactionIndex": "0x2"
            },
            {
                "address": "0x814920d1b8007207db6cb5a2dd92bf0b082bdba1",
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "data": "0x0000000000000000000000000000000000000000002de79bcc64c83a8ffcad6900000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a19f269",
                "logIndex": "0x15",
                "removed": false,
                "topics": [
                    "0xd78ad95fa46c994b6551d0da85fc275fe613ce37657fb8d5e3d130840159d822",
                    "0x000000000000000000000000145863eb42cf62847a6ca784e6416c1682b1b2ae",
                    "0x000000000000000000000000227f6757289a86c13eee2e91c2e6eb03f2ed11a6"
                ],
                "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "transactionIndex": "0x2"
            }
        ],
        "logsBloom": "0x0020080000000000000000008000000000020000000000000000000000200000000000000000000000000000010000000000000001000400000000000000400000200000008000800000000800000020000000000000000000000100000000000000040000000000000000000080000000000000000110000000001000000400000000004000c000000000000100000000000000000000081000004000000000000000000000000000200000000000000000080000080000000000000000000000000002000000001000000000000000000000000000001000000000000000000000010000000000000000000000000000000001000000000000000000000000",
        "status": "0x1",
        "to": "0x145863eb42cf62847a6ca784e6416c1682b1b2ae",
        "transactionHash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
        "transactionIndex": "0x2"
    }
}
```
