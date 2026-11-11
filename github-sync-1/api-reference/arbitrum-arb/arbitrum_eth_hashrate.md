---
description: >-
  Example code for the eth_hashrate JSON RPC method. Сomplete guide on how to
  use eth_hashrate JSON RPC  in GetBlock Web3 documentation.
---

# eth\_hashrate {disallowed} - Arbitrum

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_hashrate", "params": [], "id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
