---
description: >-
  Example code for the eth_coinbase json-rpc method. Сomplete guide on how to
  use eth_coinbase json-rpc in GetBlock.io Web3 documentation.
---

# eth\_coinbase - Binance Smart Chain

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_coinbase",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "etherbase must be explicitly specified"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
