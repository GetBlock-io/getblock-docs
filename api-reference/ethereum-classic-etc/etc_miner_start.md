---
description: >-
  Example code for the miner_start  {disallowed} json-rpc method. Сomplete guide
  on how to use miner_start  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# miner\_start {disallowed} - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "miner_start",
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
