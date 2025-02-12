---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Arbitrum

#### Parameters

`DATA` - string

Hash of a block.

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://arbitrum.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a", false],
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
        "extraData": "0xe31298ca6df96cb00d355ca9554878c19d39292b6a26eb9fc54b5df482851ef7",
        "gasLimit": "0x4000000000000",
        "gasUsed": "0x229dd8",
        "hash": "0xf5524f0cf99ac6bc5905e95294ebed9007e2d978155f3457118eb7a26d97503a",
        "l1BlockNumber": "0x105e046",
        "logsBloom": "0x00000100000000020000100000000000080000000000000000000000001000001000000000000000000000000101000000000000000000000001010000000000000010000000000000001008000120000100000000001000800100008000000000000000000000000000000000000000021000200000400000000010000000001000000000000000000000000008000000000001000000000008000000000010000000000000000000000000000000000000000000000000000000000000000000008002000048000000000000000000200800000000000010000000000000000000000040000000000000008000000010000000004010400000000000000040",
        "miner": "0xa4b000000000000000000073657175656e636572",
        "mixHash": "0x0000000000012069000000000105e046000000000000000a0000000000000000",
        "nonce": "0x00000000000c7662",
        "number": "0x5206d53",
        "parentHash": "0xb458d44871af788992e51a89661eb28a6539cf9f33c0e9a30d0aab5f2e50e974",
        "receiptsRoot": "0x184ea05480454e4c4fcf6406106a4cb13477af97d45752a126ee84de88695c7c",
        "sendCount": "0x12069",
        "sendRoot": "0xe31298ca6df96cb00d355ca9554878c19d39292b6a26eb9fc54b5df482851ef7",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x579",
        "stateRoot": "0xea41be04084dc613cfbf8d1fef75d9a097fad2b222373669e42e2aa173175649",
        "timestamp": "0x644f0242",
        "totalDifficulty": "0x3cd900b",
        "transactions": [
            "0xaf256a9ce8839489dcaa4f9a2740bcd5865b3db34a15434cce29e6bdcf043bc7",
            "0xfd7e27e19777598be2536bc6e4a5e89cc8a386eca46374333b1fba22641ed255",
            "0x98b2f04b2eb747e685c7db6f97ef2aaca236e146afb310ed039d56ab8599eb0e",
            "0x23c566cf1c2f208a74ef2e585b5bfe7fcffab6445027f06b4207ec43968cf71e",
            "0x9febdfb50d48f1891b6660379dcea3d0060808538a102d589e7ea3b7aa6d787d"
        ],
        "transactionsRoot": "0x22ef4159c9394a555af55a3ae5719bf15c1ff296838dd920f1566eeecb15aa1f",
        "uncles": []
    }
}
```
