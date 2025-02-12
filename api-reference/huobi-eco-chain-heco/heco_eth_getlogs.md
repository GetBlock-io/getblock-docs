---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Huobi ECO Chain

#### Parameters

`Object` - object

The filter options

#### Request

```java
curl --location --request POST 'https://heco.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getLogs",
"params": [{"topics": []}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "address": "0xd676d928f792165c682cd631235ad9e28abfe553",
            "blockHash": "0xc9b695b1b490b6c4257e2844c592c66608b5f06256d88402f24f72551b2be56b",
            "blockNumber": "0x18fd7a4",
            "data": "0x0000000000000000000000000000000000000000000000000000000000000000",
            "logIndex": "0x0",
            "removed": false,
            "topics": [
                "0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925",
                "0x000000000000000000000000dfb1543d74b230e9739ae1a406a73ff5a826c7b9",
                "0x000000000000000000000000a9b37a5d945f872989100ac6714d22fb707ff9d9"
            ],
            "transactionHash": "0x1d4821421147f0e37adab59d454b6f9729f1b8c98b823c1a2cf86d9ce5db89a0",
            "transactionIndex": "0x0"
        },
        {
            "address": "0xd676d928f792165c682cd631235ad9e28abfe553",
            "blockHash": "0xc9b695b1b490b6c4257e2844c592c66608b5f06256d88402f24f72551b2be56b",
            "blockNumber": "0x18fd7a4",
            "data": "0x0000000000000000000000000000000000000000000000000000000f48269ec0",
            "logIndex": "0x1",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000dfb1543d74b230e9739ae1a406a73ff5a826c7b9",
                "0x000000000000000000000000d922654715300b364e0e2ba17976198513763f80"
            ],
            "transactionHash": "0x1d4821421147f0e37adab59d454b6f9729f1b8c98b823c1a2cf86d9ce5db89a0",
            "transactionIndex": "0x0"
        },
        {
            "address": "0xa9b37a5d945f872989100ac6714d22fb707ff9d9",
            "blockHash": "0xc9b695b1b490b6c4257e2844c592c66608b5f06256d88402f24f72551b2be56b",
            "blockNumber": "0x18fd7a4",
            "data": "0x0000000000000000000000000000000000000000000000000000000f48269ec0000000000000000000000000000000000000000000000000000000000004f04d",
            "logIndex": "0x2",
            "removed": false,
            "topics": [
                "0x9ed053bb818ff08b8353cd46f78db1f0799f31c9e4458fdb425c10eccd2efc44",
                "0x000000000000000000000000dfb1543d74b230e9739ae1a406a73ff5a826c7b9",
                "0x000000000000000000000000d922654715300b364e0e2ba17976198513763f80"
            ],
            "transactionHash": "0x1d4821421147f0e37adab59d454b6f9729f1b8c98b823c1a2cf86d9ce5db89a0",
            "transactionIndex": "0x0"
        }
    ]
}
```
