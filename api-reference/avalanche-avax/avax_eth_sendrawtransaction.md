---
description: >-
  Example code for the eth_sendRawTransaction json-rpc method. Сomplete guide on
  how to use eth_sendRawTransaction json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sendRawTransaction - Avalanche

#### Parameters

`DATA` - string

The signed transaction data.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_sendRawTransaction",
"params": ["0xf86b8184843b9aca00830164e694d7ac544f8a570c4d8764c3aabcf6870cbd960d0d80844e71d92d820118a011f7e0056924be24f37b634d67dee23ef432130444cb05f7540ee03c8ce16e3ca0228e09888bc26a748ace1392d37661e90d56ec7730368ca2d55dcdb73aa69351"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "invalid sender"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
