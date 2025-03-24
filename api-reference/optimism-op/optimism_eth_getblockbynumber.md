---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Optimism

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
        "difficulty": "0x2",
        "extraData": "0xd98301090a846765746889676f312e31352e3133856c696e7578000000000000ca05166737d2fb2999d6ed01b4e652dd355340bb162bdcf8993c22593b226308620a53e3270ad738ff5d9cd8acb70d7dc3ea31f0a7ac067b1ae6845fc68cd0a901",
        "gasLimit": "0xe4e1c0",
        "gasUsed": "0x3dd27",
        "hash": "0x5ab832e53a19377023f58e567f79fe87729a5480242271d8734b31d4604872d3",
        "logsBloom": "0x0400000008000000000000800000000000000000000000000000000000000000000000000000000100000000001000000000004000000000040000000000000000004000080000000000000c00000000008000000000000000000000000000000000000000000000000000000000000000000000000000000200001008000000000200000000000008000000000000000000000002000000000000000000000000c000000000000020000004000000020000000000090000000000000000000080000002000000000000000000000000000000000000000000010000000000000000810000000000000000000000000000000000005000000000000000010000",
        "miner": "0x0000000000000000000000000000000000000000",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x6255716",
        "parentHash": "0x70e1864b1aa72bcf8a77e1dba1dda06043ced980783edd2ab7f6a79fa257b0f3",
        "receiptsRoot": "0xf7dc481d3d3fcf8125b5735bdf2ea8898230f6d1cd132753c7dc81f471a4b79b",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x65c",
        "stateRoot": "0xc5c06dd5e98b3f7a90d89fbc044c54317cca0faf5ee09345f407e397aa221d91",
        "timestamp": "0x6476c7ca",
        "totalDifficulty": "0xc4aae2d",
        "transactions": [
            "0xd0d214d179e2764b361dd2713f69b7623b693f34a72b2a4edeb609a0244fc9dc"
        ],
        "transactionsRoot": "0x2360c6d3f10377f01728ad4637822ea66d22fbc9ba8ad6eb9f96df1a1d88ceb9",
        "uncles": []
    }
}
```
