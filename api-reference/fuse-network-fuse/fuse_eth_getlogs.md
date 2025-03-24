---
description: >-
  Example code for the eth_getLogs json-rpc method. Сomplete guide on how to use
  eth_getLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getLogs - Fuse Network

#### Parameters

`Object` - object

The filter options

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
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
            "address": "0x01ab5966c1d742ae0cff7f14cc0f4d85156e83d9",
            "blockHash": "0x17adb193608bffef66890d1a9a2a5a3637b5809e58884ced1eb9af7a68cb9aef",
            "blockNumber": "0xc6e6e7",
            "data": "0x000000000000000000000000000000000000000000000000000313ba941e7200",
            "logIndex": "0x0",
            "removed": false,
            "topics": [
                "0xf60e56b8021f6ac77bd50d4acdbed9ad9e85107144d43090774a9ebe5ffed985",
                "0x0000000000000000000000000bf5bae7cc3e5e014d990623a1bf68f61f99d36f"
            ],
            "transactionHash": "0xe31766264ce60e0909886b551b66566f0d647addda662f93734a7768e27a6d51",
            "transactionIndex": "0x1",
            "transactionLogIndex": "0x0",
            "type": "mined"
        },
        {
            "address": "0x495d133b938596c9984d462f007b676bdc57ecec",
            "blockHash": "0x17adb193608bffef66890d1a9a2a5a3637b5809e58884ced1eb9af7a68cb9aef",
            "blockNumber": "0xc6e6e7",
            "data": "0x0000000000000000000000000000000000000000000000000000000000000ab6",
            "logIndex": "0x1",
            "removed": false,
            "topics": [
                "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
                "0x000000000000000000000000d7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
                "0x0000000000000000000000005c45bf97fa6f3502ba38172933ea26d82773e1a6"
            ],
            "transactionHash": "0x81e3215a127ca12dd860bf385e04cc05f8ca14022d22b3b22635309d8173dab7",
            "transactionIndex": "0x2",
            "transactionLogIndex": "0x0",
            "type": "mined"
        }
    ]
}
```
