---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Avalanche

#### Parameters

`DATA` - string

Hash of a block.

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_getBlockByHash",
  "params": [
    "0x3e56c97d34f03b1369c351fa6c9f57c8bfa987c7da40964fab981303e0ef5849",
    false
  ],
  "id": "getblock.io"
}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": "0x142cbb9752",
        "blockExtraData": "0x",
        "blockGasCost": "0x30d40",
        "difficulty": "0x1",
        "extDataGasUsed": "0x0",
        "extDataHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "extraData": "0x000000000046ee750000000000153605000000000000000000000000000000000000000000291fa700000000000000000000000000313f7d000000000000000000000000003375a50000000000000000",
        "gasLimit": "0x7a1200",
        "gasUsed": "0x32935a",
        "hash": "0x3e56c97d34f03b1369c351fa6c9f57c8bfa987c7da40964fab981303e0ef5849",
        "logsBloom": "0x002000000000010000060000a000000000020000a000400020040090050000000200004008110000084000001202400000000002000a80100000000000210000010080000048000480006008100008301000000a00d604004000110280200400002000000210040000000009108048000040800000800400300000116200000044000820000010008000000000000000020008010001000a01000060002000020248040042000004000002042900000000000008a000000021000940000420014000005220040008200010004004081000008000111400300800000240003000009098088000081800030004002000480c000400040000710000002040189008",
        "miner": "0x0100000000000000000000000000000000000000",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0xca8de4",
        "parentHash": "0x2dd9b8a134be8e38e0275bb0d9c7c79226449c1143db2ee79be0820bdc07483e",
        "receiptsRoot": "0x2fa6627537c3cf623930d6de42151c6a6968476c1a0aa0e8ba343b8a5d4a07bf",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x19c0",
        "stateRoot": "0xc53441430d92f267c13fb844f7f3f23bdeee1aefdf2980e0d8f4f16d884af2f4",
        "timestamp": "0x6253f7b2",
        "totalDifficulty": "0xca8de4",
        "transactions": [
            "0x3dee39f608a0da41254143baf1d709ff1fae84150b57b3e037d206946826161e",
            "0xcf21fc4b3b91a9aa0f78ad57eb9e2f1e3718de6430b063e601a5c2b3b81d8cc8",
            "0x67786eab7b0c84822285a5fa03f5d696a0ead38fb69fc929908fe20f7bcce65f",
            "0x22dc79ee594717cf14a20f125906b658d88f595a4409afb71fd85dc0cb7c180c",
            "0xfc1dd0655a2e7a4f863de5513df670677ce66b2c06a6cefbfb81be466b126b0b",
            "0x0686935e259f22e5278c1a465a1977eeb9e5734389a56772bcdd9ef0434a8a7a",
            "0xc7dba5a363e4a9d09c354b271b9a41175ad924e983ce62bd1c02d4b050443771",
            "0x55ed25d57863494a5b575713e906b1f5a53f9c1d8480378f546658b910652e0f",
            "0xfff8a862d666fd5e89fe39eedee7c5a1fd7b7f2d421f16815faa0bb3b61e2758",
            "0xf00b8e320a42e22d8f315abd1e7e6e2f16fb870ea8f2691cbe5b5a3f0f2ed1a6",
            "0x2663d7488d16454470867de3c9c0d275a5546d0f78926a73b76f34624d29bc09",
            "0xc1a05782486d07e955a822558b04b3daa4e7440d7d40aaf46ac984a5e3ffb890",
            "0xd9f415566058213cfa5570b0693da779a97bfdb1e2f3cd660639f431bc4749f2",
            "0x75f5d06784ea5c14264e662b58bc96a4510c56817d32a5dae7a32a35800e992c",
            "0x77184c19b749aff07f9371392daed16e5ab5525f55f13cf5a79f0383f4448222",
            "0x323af1c83becd5e035f731f31bbba5ec2ebe63b0fc75c80f454e2920f7de3ae0"
        ],
        "transactionsRoot": "0x08e12bae3792bc3179a45c2decefd916674b27e6accbb00931781cf586c6706e",
        "uncles": []
    }
}
```
