---
description: >-
  Example code for the admin.lockProfile  {disallowed} json-rpc method. Сomplete
  guide on how to use admin.lockProfile  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# admin.lockProfile {disallowed} - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://avax.getblock.io/mainnet/ext/bc/C/rpc' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "admin.lockProfile",
"params": [],
"id": "getblock.io"}'
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
