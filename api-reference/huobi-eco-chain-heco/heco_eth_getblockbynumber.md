---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Huobi ECO Chain

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://heco.getblock.io/mainnet/' 
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
        "baseFeePerGas": "0x0",
        "difficulty": "0x2",
        "extraData": "0xd883010202846765746888676f312e31392e35856c696e7578000000000000007e6193b9ff6e91d632f5e7b2b5de25d081d37480bd3c594a58b8fcd0e0fc21aa596483651a8260cd6c197f9fe54dbe04bb84e9b833162d7317613c3e84b804a000",
        "gasLimit": "0x2625a00",
        "gasUsed": "0x54ab4",
        "hash": "0xf8ae1585491bd0e646fc3578b3fb80c0000d436ca76ae67d22966cdc849d59cc",
        "logsBloom": "0x00200000000040000000000080000001000000000280000000000004000000000000100000000000000400000000000000000000040000008000000000240000000001000000000000000008000000200000000808000000000000000000000000000000000000400000000000000000000000000004000000800010480002000000000000000048000000000010000000000000000200080000004000000000020000408000000000000000000100000000000000800000000000001010000080000002000000020000000800000000000000000000001000800000000000000010000000400800000000000000000000000000000000000000000000000000",
        "miner": "0xb67147cc85798a08641fb2bd58aff9ca4dd3f865",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x18fd7a3",
        "parentHash": "0x5524c881ef1f23d1ef3ce85283482e819232f13c28bd71ab317fd92954a3aad3",
        "receiptsRoot": "0xbb74dd002f390c43e7a1686503dec55619ec333a7692ae00cb2c18f5aa5d91fe",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x4b4",
        "stateRoot": "0xadc2520e0c3c87cb4dd10f59bb0b0a3dd7648f6bc4c557778d548f172366fe51",
        "timestamp": "0x6476cc89",
        "totalDifficulty": "0x31f5a44",
        "transactions": [
            "0x44abdbe13444988567be10029f366df275f2abac11992dcca22edcb0aff4a5b8",
            "0x885093ccc7a6ce5b626ea6e7625ef646431afb6212a7d19ec99c5a5192c41d69"
        ],
        "transactionsRoot": "0x07e17a869e6a042fcfde7b53ad9341cbe3a896009848b7237371992fc92d27c1",
        "uncles": []
    }
}
```
