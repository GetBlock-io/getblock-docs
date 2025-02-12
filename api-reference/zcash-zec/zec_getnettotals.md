---
description: >-
  Example code for the getnettotals  {disallowed} json-rpc method. Сomplete
  guide on how to use getnettotals  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getnettotals {disallowed} - Zcash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getnettotals",
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
