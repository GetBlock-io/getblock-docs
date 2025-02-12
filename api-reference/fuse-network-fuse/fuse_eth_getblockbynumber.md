---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Fuse Network

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://fuse.getblock.io/mainnet/' 
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
        "author": "0xd6fd490784d1fbb54df0c13494e2cbe5fbf25f03",
        "difficulty": "0xfffffffffffffffffffffffffffffffe",
        "extraData": "0xdb830303058c4f70656e457468657265756d86312e35392e30826c69",
        "gasLimit": "0x1312d00",
        "gasUsed": "0x4fc6f",
        "hash": "0x89b1661295ad3d8c40c513b7aac32d36df415969e9e6f26ebc2756e6942980dd",
        "logsBloom": "0x00000000000000000000002000000000000000000000000000000020001000000000000000000000000000000000000000040000000000200000000000010000000000000000040000000008000000000000000000000000000000000100000000000000000000000000000000000000000000000010000000000010000000000004000000000000000000000000000000000000201000010000000000000000800000000000000000000000000000000000000000001000004000000000100000000002000000000000800000000200000000000008000000000000000000000000000000000000000001000088000000000000000000000008000000000000",
        "miner": "0xd6fd490784d1fbb54df0c13494e2cbe5fbf25f03",
        "number": "0x167cdfd",
        "parentHash": "0x462dfa93ebf8858bf4a358fe214cb54b818587ed02615b46ec36486f87bd237d",
        "receiptsRoot": "0x7bb583fd3699fb495be705631cd96aec7c8859413668d34acaf2110ba87349c8",
        "sealFields": [
            "0x841417c2ca",
            "0xb84180ef3f226d3a68e3fe8c3f91d5beecef5e4468180dc27cfc280c1d9fdd07a0025bdcb4ae1d6425364c2c86188ec136c83acebd703c0ebaf6a9c7eb2047387aca00"
        ],
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "signature": "80ef3f226d3a68e3fe8c3f91d5beecef5e4468180dc27cfc280c1d9fdd07a0025bdcb4ae1d6425364c2c86188ec136c83acebd703c0ebaf6a9c7eb2047387aca00",
        "size": "0x34b",
        "stateRoot": "0x0142656813005af560fe69add32e9b3566f5bf8c1cfdbd20e0e73c98267c9a00",
        "step": "337101514",
        "timestamp": "0x6476cdf2",
        "totalDifficulty": "0x167cdfcffffffffffffffffffffffffea826f39",
        "transactions": [
            "0x4a868765061a4254ea3f1c7b711f683cdd1ec2898fcf714583049a741a15c082",
            "0xfada592029fa5c8a76885193a74036060bbc868bbe7aabe69180ce717299e463"
        ],
        "transactionsRoot": "0x4c4605c6f4e717c9fa4fc20d7b2a238b32a26b0062d1bb4e551ffed1f5523329",
        "uncles": []
    }
}
```
