---
description: >-
  Example code for the eth_getUncleByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getUncleByBlockNumberAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getUncleByBlockNumberAndIndex - Gnosis

#### Parameters

`quantity|tag` - hex string

Index of the block, or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`quantity` - hex string

Index of the uncle.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleByBlockNumberAndIndex",
"params": ["0x89D2", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32602,
        "message": "Position Index is incorrect"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
