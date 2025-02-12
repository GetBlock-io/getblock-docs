---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Huobi ECO Chain

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

`QUANTITY` - integer

Transaction index position.

#### Request

```java
curl --location --request POST 'https://heco.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["latest", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xf8ae1585491bd0e646fc3578b3fb80c0000d436ca76ae67d22966cdc849d59cc",
        "blockNumber": "0x18fd7a3",
        "from": "0x7bbf1380358195db198516d0fbaa32c4f851abec",
        "gas": "0x2a9f0",
        "gasPrice": "0x938580c0",
        "hash": "0x44abdbe13444988567be10029f366df275f2abac11992dcca22edcb0aff4a5b8",
        "input": "0xa9059cbb000000000000000000000000d2b78a0e6ce11ac1c2a1500c772a65129a49b5e200000000000000000000000000000000000000000000d3c21bcecceda1000000",
        "nonce": "0x11f2",
        "r": "0x349c04355855c6094a280cea297bd52aaea76909b1a938ddd51ce852174924a9",
        "s": "0x5f4e51af2e5a8a993acc2085ad74f1370c8de719e2a4621fa2968b1566479c82",
        "to": "0x86a48017e7e11c6d641e9157dffc677b81449d9b",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x123",
        "value": "0x0"
    }
}
```
