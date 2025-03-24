---
description: >-
  Example code for the eth_getUncleCountByBlockHash json-rpc method. Сomplete
  guide on how to use eth_getUncleCountByBlockHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getUncleCountByBlockHash - Ethereum Classic

#### Parameters

`DATA` - string

32-byte block hash.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleCountByBlockHash",
"params": ["0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0"
}
```
