---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Gnosis

#### Parameters

`DATA` - hex string

32-byte hash of a block.

`Boolean` - boolean

If true, returns the full transaction objects; if false, returns the transaction hashes.

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0xc11de1b67dd435ada2b83bbf0bb45cba5efab4e8dd00b100142e799ba902dddc", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "author": "0x5112d584a1c72fc250176b57aeba5ffbbb287d8f",
        "baseFeePerGas": "0x7",
        "difficulty": "0x0",
        "extraData": "0x4e65746865726d696e64",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0x5695f",
        "hash": "0xc11de1b67dd435ada2b83bbf0bb45cba5efab4e8dd00b100142e799ba902dddc",
        "logsBloom": "0x00000001000000000000002042000000200000000810000000000000040000000000c00000000000000004002000020000000000000008010000000000200000800000000000000000020001000020000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000002000000000018000000000000000000020000000000000000010000000020000000000000000000000000000200000000000000800000000000000000000000000000000000000000000000000100000010000000002000000010000000000000000000000000000000000010400000",
        "miner": "0x5112d584a1c72fc250176b57aeba5ffbbb287d8f",
        "mixHash": "0x0706309ed32e224d19630cbfdc74bcf642854458c494155e15025636b66e29e8",
        "nonce": "0x0000000000000000",
        "number": "0x1ac46e3",
        "parentHash": "0x41471bdfd80b1b5f5d13213b45e45ed6d4d56a54df1b3b196c79dcedd07e7d70",
        "receiptsRoot": "0x251a1d57debdc17dde16815c5384392d1729412f53df5bab974f3cfd45a1aa5c",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x6f9",
        "stateRoot": "0x61805ee6e6ab79feab2b7a913f8a0207ec2b4a1437317192177029aefbb60359",
        "timestamp": "0x646b0a08",
        "totalDifficulty": "0x182cd9fffffffffffffffffffffffffea9528a2",
        "transactions": [
            "0xc6b20f02936f72709b65026d55f425cd7235f314b5e95a61bc21e54b2ef1fd49",
            "0xbeaaab95ee2a73c51adb3ff679c4c561b7a10f9d2555862a77d82d84dbf47748",
            "0x1ed48327deb9fe6b62d6c17bb3643fdd57e8d5245dcf1ded2054772adb0a9735"
        ],
        "transactionsRoot": "0x9a968e320a2ac5e02ba3e59bbad0ca3203cd007d1136a09beb2f4eb827e91438",
        "uncles": []
    }
}
```
