---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Moonbeam

#### Parameters

`DATA` - string

Hash of a block.

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0xe21d18d5b511b13c66b4d162111b5d41646782378ee35eb208b62be36942a859", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "author": "0xecca07badbd38937122b82ec8afcf86b1e2b7939",
        "baseFeePerGas": "0x1d9e8ae025",
        "difficulty": "0x0",
        "extraData": "0x",
        "gasLimit": "0xe4e1c0",
        "gasUsed": "0x0",
        "hash": "0xe21d18d5b511b13c66b4d162111b5d41646782378ee35eb208b62be36942a859",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "0xecca07badbd38937122b82ec8afcf86b1e2b7939",
        "nonce": "0x0000000000000000",
        "number": "0x3752d1",
        "parentHash": "0x13d5a7a05b59472acd9074fd67733fa297f32f5d516cbc20d337d1b5e5b4558d",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x201",
        "stateRoot": "0xd1b634b886c999de96df2417c5f925f20f0e35b80c749d2bef15f0f30d354a82",
        "timestamp": "0x646d0dfc",
        "totalDifficulty": "0x0",
        "transactions": [],
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "uncles": []
    }
}
```
