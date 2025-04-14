---
description: >-
  Example code for the eth_mining json-rpc method. Сomplete guide on how to use
  eth_mining json-rpc in GetBlock.io Web3 documentation.
---

# eth\_mining - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \ 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_mining",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32601,
        "message": "the method eth_mining does not exist/is not available"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
