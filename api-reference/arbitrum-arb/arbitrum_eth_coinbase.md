---
description: >-
  Example code for the eth_coinbase JSON RPC method. Сomplete guide on how to
  use eth_coinbase JSON RPC in GetBlock.io Web3 documentation.
---

# eth\_coinbase {disallowed} - Arbitrum

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0", "method": "eth_coinbase", "params": [], "id": "getblock.io"}'
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
