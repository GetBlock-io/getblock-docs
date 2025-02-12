---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Optimism

#### Parameters

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

`QUANTITY` - string

Transaction index position.

#### Request

```java
curl --location --request POST 'https://optimism.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["0xc6f437", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xc4c7200bfc5fd0b52dc6af319820269bcdab29da859da2657922f57e1de35ca0",
        "blockNumber": "0xc6f437",
        "from": "0x0ef629d2b5bcab52293ba2cb5e42088bfe755ffa",
        "gas": "0x435b9",
        "gasPrice": "0xf4240",
        "hash": "0xf722d4c6c9fbef0b8ef72216f10d84123466705d970f7bb0e82f99d7dc5b746a",
        "index": "0xc6f436",
        "input": "0xeea0d7b2000000000000000000000000000000000000000000000000000000000000a4b10000000000000000000000000ef629d2b5bcab52293ba2cb5e42088bfe755ffa000000000000000000000000000000000000000000000000026a2c13feeb0000000000000000000000000000000000000000000000000000000193fd1edc42d300000000000000000000000000000000000000000000000002662a419c0b338e0000000000000000000000000000000000000000000000000000000062c03feb0000000000000000000000000000000000000000000000000259d66702bd9add0000000000000000000000000000000000000000000000000000000062c03feb",
        "l1BlockNumber": "0xe53e76",
        "l1Timestamp": "0x62b7058c",
        "l1TxOrigin": null,
        "nonce": "0x9",
        "queueIndex": null,
        "queueOrigin": "sequencer",
        "r": "0xf6509cbf93d184009993958a8b2f5967e6b5fb792e12385f2d9c4dc8f8d24740",
        "rawTransaction": "0xf9017109830f4240830435b99486ca30bef97fb651b8d866d45503684b90cb331288026a2c13feeb0000b90104eea0d7b2000000000000000000000000000000000000000000000000000000000000a4b10000000000000000000000000ef629d2b5bcab52293ba2cb5e42088bfe755ffa000000000000000000000000000000000000000000000000026a2c13feeb0000000000000000000000000000000000000000000000000000000193fd1edc42d300000000000000000000000000000000000000000000000002662a419c0b338e0000000000000000000000000000000000000000000000000000000062c03feb0000000000000000000000000000000000000000000000000259d66702bd9add0000000000000000000000000000000000000000000000000000000062c03feb38a0f6509cbf93d184009993958a8b2f5967e6b5fb792e12385f2d9c4dc8f8d24740a042d899f25c72c4ce8ba6eb0056f17f9a8ffabe5067d374e9d0d64306b54bcaf0",
        "s": "0x42d899f25c72c4ce8ba6eb0056f17f9a8ffabe5067d374e9d0d64306b54bcaf0",
        "to": "0x86ca30bef97fb651b8d866d45503684b90cb3312",
        "transactionIndex": "0x0",
        "v": "0x38",
        "value": "0x26a2c13feeb0000"
    }
}
```
