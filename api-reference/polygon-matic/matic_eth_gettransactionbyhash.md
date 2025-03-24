---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Polygon

#### Parameters

`DATA` - string

Hash of a transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0xe765898fc5cd4835df585df5b913456f274c35287e29057366a8d84d8c9fc8e3"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x81e807e7a6031d9f103eeee2a2edc5994c3432ee1e3227c66ff78eef30ea1dec",
        "blockNumber": "0x1236b4b",
        "from": "0x75d56b0103ea3b596d344e153cc89012af3bd22a",
        "gas": "0x5bd0e",
        "gasPrice": "0x1d1a94a2000",
        "hash": "0xe765898fc5cd4835df585df5b913456f274c35287e29057366a8d84d8c9fc8e3",
        "input": "0x79bc47252a050001050001010000000000000005000000000000000000000000000001db0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000319b0b48626f2ea8000",
        "nonce": "0x161",
        "r": "0xd1775131ba56ab8b2306e552700ee3050a20b2b751537386464d80daaaad9950",
        "s": "0x4055049844c70c8187abd97b50b18ebe296936eacb613e49a5d72c46ab253ff2",
        "to": "0x013b4f7e1c988d428a2183bcf158d2584f76a6d7",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x135",
        "value": "0x0"
    }
}
```
