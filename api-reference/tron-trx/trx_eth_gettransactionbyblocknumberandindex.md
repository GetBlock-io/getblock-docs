---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - TRON

#### Parameters

`QUANTITY|TAG` - QUANTITY|TAG

a block number, or the string "earliest", "latest"

`QUANTITY` - string

the transaction index position

#### Request

```java
curl --location --request POST 'https://trx.getblock.io/mainnet/jsonrpc' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["latest", "0x1"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x00000000025ad7ec652741485c5410bcfb36ae1047d9ef617b406cec8f87f3c6",
        "blockNumber": "0x25ad7ec",
        "from": "0xf1b07b02c1d0df1809d67b31d91f1f37a99e52b9",
        "gas": "0xb288",
        "gasPrice": "0x118",
        "hash": "0x7c5350ac4ceb9c61e1470feed2a98cb195d6e95e96a8802682fcfc96fdc74c6c",
        "input": "0x",
        "nonce": null,
        "r": "0xe7a050b171fe286454cbc44bf8b28cff15d8059944b9b8a16d66fe53e55ebd1f",
        "s": "0x1fd9aa5f5e678ffdfdd1b6730e1fc758fd5aeacd4fc4f103056e55959fbc5800",
        "to": "0x1e71ec9329412fc1c0e8feb6444dfa0ab28be078",
        "transactionIndex": "0x1",
        "type": "0x0",
        "v": "0x1b",
        "value": "0x0"
    }
}
```
