---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Moonbeam

#### Parameters

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
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
        "author": "0xe751b9ea560a200161d1b70249495e3d22ec5b00",
        "baseFeePerGas": "0x1e471fd76d",
        "difficulty": "0x0",
        "extraData": "0x",
        "gasLimit": "0xe4e1c0",
        "gasUsed": "0xa92659",
        "hash": "0x9b29489bad8ece67542c9e0f125731d55462434f3fed85fdf74e56f52d4d05a3",
        "logsBloom": "0x003000022000100000050c2810200008020000000840000400081470480880500000000000000204100800010010800000003049000021060104010084200052000015c04000808a8000000c084004080004800006400240000600004482000730011030020280a000c0802110000840184480400022c6012802013020880001410200030150080000800006004040278000201c1001040860000000008a0000260000410422406800c0000020044548800210070b0000010004402c00700210540000160804004642a0280000c21200000060a001410410200240228140240001304101200000a094080008000110a002400008404000084844400020200600",
        "miner": "0xe751b9ea560a200161d1b70249495e3d22ec5b00",
        "nonce": "0x0000000000000000",
        "number": "0x381c83",
        "parentHash": "0xaef97f180cf09aad3b785d2b8150ef7d342be04aeb01f2bc25efcdf32465cbef",
        "receiptsRoot": "0xc336ced86309460aca970410c8442b5ac5bb10f491da71e5a685be62604dc931",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0xc25",
        "stateRoot": "0x1e58ec43c0882343feb921c44ed8185c062ec6dedb58cf25c63f055b3f90ce41",
        "timestamp": "0x6476b788",
        "totalDifficulty": "0x0",
        "transactions": [
            "0xc372f85b8de96c21572f959796ae82eb2eb8b577e2d9475fbbb57648c371e81a",
            "0x1256e2ab871700997e6f9a80d06f90dd1b6dea1d472f5b5f49ba02298917f148",
            "0xc53d007edb6f3d7f7b0b61ceb1604b23be9d9f16216404d5909ddba34e303aa0",
            "0x01b510471da6469165bfc9ce2bae056676c217a4e80674f9056a73a6e094f8e4"
        ],
        "transactionsRoot": "0x9fb6b3628b68fa2a376ec406a5bbea58e09f1f9023fbbd82c35fc8f237dcf595",
        "uncles": []
    }
}
```
