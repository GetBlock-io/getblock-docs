---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Arbitrum

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://arbitrum.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByNumber",
"params": ["latest", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": "0x5f5e100",
        "difficulty": "0x1",
        "extraData": "0xd79c44e4c3397b74b78db0ea2d34c1106d8bae0752d6e352a859ae27185dec62",
        "gasLimit": "0x4000000000000",
        "gasUsed": "0x9b0c67",
        "hash": "0x9a682d3983aa3c86a32cc87ca21ae5f5eb7b5d175155ed941238e60b681ad280",
        "l1BlockNumber": "0x10924a4",
        "logsBloom": "0x0808000400200000c0040000000000840000000100040200400000000000000c010002000000000006204000012000080000008000100000000040000000000000102021000000008000006800000000000000000002010010004001048004000000000003000000000100200000080000000000800080000000201004000000000000000000800000000000000008088002000400002000000000400800000002000001008000000008000000000080000020020000004000020000001000000000800200000800060820000081000209000c000084020400010100008020010000001400400002000000080000400040001024016004000000000000000000",
        "miner": "0xa4b000000000000000000073657175656e636572",
        "mixHash": "0x000000000001310800000000010924a4000000000000000a0000000000000000",
        "nonce": "0x00000000000d7b9a",
        "number": "0x5bc4077",
        "parentHash": "0x6becdeda3dd59cbd1d5a459407ba3ffc298a1c64c7af27756f43d0db21144a92",
        "receiptsRoot": "0x18dd23ed85557f5ddf1075d35012611580401daf7ecb8a906915ef2036e9425c",
        "sendCount": "0x13108",
        "sendRoot": "0xd79c44e4c3397b74b78db0ea2d34c1106d8bae0752d6e352a859ae27185dec62",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x2d3a",
        "stateRoot": "0xb7845bcebb2a06b61b66e468c4f28ffa73e281101113b32bb49f1fb445d6eb8f",
        "timestamp": "0x6476c9df",
        "totalDifficulty": "0x469632f",
        "transactions": [
            "0x44613c8636c24b26ef754c899cb5f48600ccfe13a0ee697b40a9e92a0cb25a72",
            "0x0543ea3f502977d188eff073db26a325c0dec7f72f7a5033e65b6728964e2231"
        ],
        "transactionsRoot": "0x7989d9339de2ea4c5434ab01c0d5aff3853d6a0d746f306562ebe7ecb49e2c74",
        "uncles": []
    }
}
```
