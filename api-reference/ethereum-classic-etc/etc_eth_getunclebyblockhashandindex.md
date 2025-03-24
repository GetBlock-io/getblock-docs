---
description: >-
  Example code for the eth_getUncleByBlockHashAndIndex json-rpc method. Сomplete
  guide on how to use eth_getUncleByBlockHashAndIndex json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getUncleByBlockHashAndIndex - Ethereum Classic

#### Parameters

`data` - string

32-byte block hash.

`quantity` - string

Index of the uncle.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleByBlockHashAndIndex",
"params": ["0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": null
}
```
