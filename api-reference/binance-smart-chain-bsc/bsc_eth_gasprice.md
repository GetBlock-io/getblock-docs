---
description: >-
  Example code for the eth_gasPrice json-rpc method. Сomplete guide on how to
  use eth_gasPrice json-rpc in GetBlock.io Web3 documentation.
---

# eth\_gasPrice - Binance Smart Chain

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_gasPrice",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xb2d05e00"
}
```
