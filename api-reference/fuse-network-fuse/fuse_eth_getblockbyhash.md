---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Fuse Network

#### Parameters

`DATA` - string

Hash of a block.

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://fuse.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0x360c79467da57c8db35624484cabbca4d9fc7a7813f194f5391639d2e2c39023", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "author": "0x9b956e3d318625be2686ae7268d81777c462d41f",
        "difficulty": "0xfffffffffffffffffffffffffffffffe",
        "extraData": "0xde8302050d8f5061726974792d457468657265756d86312e34302e30826c69",
        "gasLimit": "0x989680",
        "gasUsed": "0x0",
        "hash": "0x360c79467da57c8db35624484cabbca4d9fc7a7813f194f5391639d2e2c39023",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "0x9b956e3d318625be2686ae7268d81777c462d41f",
        "number": "0xc63463",
        "parentHash": "0x8f0e7967056b9e58673df9fa3a4d2fb79e84f6c1865555965b7d997d10410181",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "sealFields": [
            "0x84137415ec",
            "0xb841b4dc56f18f9c038e76aac442c86815fc53c76e0370efaf356e4b6a35e3769c435ce9adf6d1573e39d17d2436aab3ba530c1517e6282406a7f58271b58011be0100"
        ],
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "signature": "b4dc56f18f9c038e76aac442c86815fc53c76e0370efaf356e4b6a35e3769c435ce9adf6d1573e39d17d2436aab3ba530c1517e6282406a7f58271b58011be0100",
        "size": "0x24c",
        "stateRoot": "0xa23efd29bc734f76b9107b2c774bf473cc7ccb80b669a7c338e716938af97e58",
        "step": "326374892",
        "timestamp": "0x61446d9c",
        "totalDifficulty": "0xc63462ffffffffffffffffffffffffebc7b5b1",
        "transactions": [],
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "uncles": []
    }
}
```
