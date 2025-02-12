---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockHashAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - KuCoin Community Chain

#### Parameters

`DATA` - string

Hash of a block.

`QUANTITY` - string

Transaction index position.

#### Request

```java
curl --location --request POST 'https://kcc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockHashAndIndex",
"params": ["0x56436a935370b8decda78cf4a60e0668a882764af1cdec3a6d6967f944f4dace", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x56436a935370b8decda78cf4a60e0668a882764af1cdec3a6d6967f944f4dace",
        "blockNumber": "0xc6f437",
        "chainId": "0x7a",
        "condition": null,
        "creates": null,
        "from": "0xed228c1c87c9ecb44ea640fc8b0f4955e4296233",
        "gas": "0x164e6",
        "gasPrice": "0x3b9aca00",
        "hash": "0xb036e4ff7e7f596615d121d3a1c923c618bd3c2d3745c512346d7d8e583f01f7",
        "input": "0x4e71d92d",
        "nonce": "0x84",
        "publicKey": "0x7e96b8f2e31b2e27d26cded3f40b7ef1f9661767971caf1095445cf40ceec2f9b4c58781fd1759559f0a6ca50b7960861ca2c9c8e153b5186dddc9f2027ecf68",
        "r": "0x11f7e0056924be24f37b634d67dee23ef432130444cb05f7540ee03c8ce16e3c",
        "raw": "0xf86b8184843b9aca00830164e694d7ac544f8a570c4d8764c3aabcf6870cbd960d0d80844e71d92d820118a011f7e0056924be24f37b634d67dee23ef432130444cb05f7540ee03c8ce16e3ca0228e09888bc26a748ace1392d37661e90d56ec7730368ca2d55dcdb73aa69351",
        "s": "0x228e09888bc26a748ace1392d37661e90d56ec7730368ca2d55dcdb73aa69351",
        "standardV": "0x1",
        "to": "0xd7ac544f8a570c4d8764c3aabcf6870cbd960d0d",
        "transactionIndex": "0x0",
        "v": "0x118",
        "value": "0x0"
    }
}
```
