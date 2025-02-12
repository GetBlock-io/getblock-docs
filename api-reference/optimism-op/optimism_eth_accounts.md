---
description: >-
  Example code for the eth_accounts json-rpc method. Сomplete guide on how to
  use eth_accounts json-rpc in GetBlock.io Web3 documentation.
---

# eth\_accounts - Optimism

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://optimism.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_accounts",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        "0x00000398232e2064f896018496b4b44b3d62751f"
    ]
}
```
