---
description: >-
  Example code for the debug_metrics  {disallowed} json-rpc method. Сomplete
  guide on how to use debug_metrics  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# debug\_metrics {disallowed} - Binance Smart Chain

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_metrics",
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
