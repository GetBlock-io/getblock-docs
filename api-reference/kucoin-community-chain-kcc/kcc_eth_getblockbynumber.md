---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - KuCoin Community Chain

#### Parameters

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://kcc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
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
        "difficulty": "0x2",
        "extraData": "0xd98301090a846765746889676f312e31352e3133856c696e7578000000000000449ef55c364234db32ee6234386930c064b664826f35f7657012d5283966710b6c20e642f8b5524101c6873803972f51bbd37d579a31c860e7e1bd87b873a4b801",
        "gasLimit": "0xa7d8c0",
        "gasUsed": "0x125464",
        "hash": "0x723a9cebdda2926a99880e330ec286452dc1698e78f90f10c976e82eee51a8b1",
        "logsBloom": "0x00000000000000000000000010000000000000200000000000040000008000000000000000000000000000100000000100000000000001000000000000000000100000000002000000000008000000000800000000000000000000400000000000000000000000000000000000000004400000000400000000000090100000000000000000000400000000000000000000000040002000000008000000000000000000000000000000000000000000000000000000000000000000002000000000000002000000000100000000000088000000000000000000000000000000000000000000000008000000000002000000000000000000000000000000400000",
        "miner": "0x0000000000000000000000000000000000000000",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x2008db",
        "parentHash": "0x0c0a7cb4efcef1976adf881ad3fea8c2cf9d06d85abe73f17b3a2bc8053a4f10",
        "receiptsRoot": "0x85a6093bd39a853039a5316b8dbe560218ea9f3f51e23b28e396bf9df3e7e116",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x313",
        "stateRoot": "0x0ccf156a17b95e08e3b5e6d3b9d415479fb38ece5e71075ccb77cebeadc696f7",
        "timestamp": "0x61499aa6",
        "totalDifficulty": "0x4011b7",
        "transactions": [
            "0x2ff8a77fd1424b031ceadbc831a0fb4cfe117b58efc1703fc489aadacce5e994"
        ],
        "transactionsRoot": "0xeb9e697d23a3e6f18b744e780b6068f45480cecef2a052a6bab6656f72631db7",
        "uncles": []
    }
}
```
