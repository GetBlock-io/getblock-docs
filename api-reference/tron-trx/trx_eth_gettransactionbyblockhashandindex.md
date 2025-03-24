---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - TRON

#### Parameters

`hash` - DATA, 20 bytes

hash of a block

`QUANTITY` - string

the transaction index position

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockHashAndIndex",
"params": ["0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f", "0x1"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x00000000025ad523bf3d34c17ff81e2cf46824d38ff0373d172fd293ca7e651f",
        "blockNumber": "0x25ad523",
        "from": "0x0b48984414cc0c6a8e599fb6e3bc11e599de2e24",
        "gas": "0x3927",
        "gasPrice": "0x118",
        "hash": "0x144addd6d19590a5eef5ae03d45a41b9628191993b33d37b450d7fdf89525926",
        "input": "0x",
        "nonce": null,
        "r": "0x203115ef3e77385bf311be90f6c61b0e3f5048ecc27d1b4b4539df4bacadc9aa",
        "s": "0x23dcda1c3a6a575d04c161f2d990d26232d2977be1de8848b95876f9a1ece307",
        "to": "0xa614f803b6fd780986a42c78ec9c7f77e6ded13c",
        "transactionIndex": "0x1",
        "type": "0x0",
        "v": "0x1b",
        "value": "0x0"
    }
}
```
