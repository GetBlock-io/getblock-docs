---
description: >-
  Example code for the rpc_modules json-rpc method. Сomplete guide on how to use
  rpc_modules json-rpc in GetBlock.io Web3 documentation.
---

# rpc\_modules - Optimism

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "rpc_modules",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "debug": "1.0",
        "eth": "1.0",
        "net": "1.0",
        "rollup": "1.0",
        "rpc": "1.0",
        "web3": "1.0"
    }
}
```
