---
description: >-
  Example code for the eth_mining JSON RPC method. Сomplete guide on how to use
  eth_mining JSON RPC in GetBlock Web3 documentation.
---

# eth\_mining {disallowed} - Arbitrum

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/YOUR-ACCESS-TOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0", "method": "eth_mining", "params": [], "id": "getblock.io"}'
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
