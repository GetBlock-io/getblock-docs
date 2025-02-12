---
description: >-
  Example code for the getSnapshotSlot  {disallowed} json-rpc method. Сomplete
  guide on how to use getSnapshotSlot  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getSnapshotSlot {disallowed} - Solana

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://sol.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getSnapshotSlot",
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
