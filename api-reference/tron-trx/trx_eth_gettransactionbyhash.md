---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - TRON

#### Parameters

`hash` - bytes 32

hash of a transaction

#### Request

```java
curl --location --request POST 'https://trx.getblock.io/mainnet/jsonrpc' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0x9ff75c018157f4504adc34058510516c603569238e5c3d156b1f0b096856b1a1"],
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
        "from": "0xa346f2bd7d43a5d90b7c57a18196be96b2840e61",
        "gas": "0x73bf",
        "gasPrice": "0x118",
        "hash": "0x9ff75c018157f4504adc34058510516c603569238e5c3d156b1f0b096856b1a1",
        "input": "0x",
        "nonce": null,
        "r": "0xa860b1738e436d5b6d41b904abeafb36b210f9d839501e22daa2df61b606fe46",
        "s": "0x0a10bb9debcafcf921960a42c17ebb1787d3bde6e46c692b0cac66f190549164",
        "to": "0xa614f803b6fd780986a42c78ec9c7f77e6ded13c",
        "transactionIndex": "0x3e",
        "type": "0x0",
        "v": "0x1b",
        "value": "0x0"
    }
}
```
