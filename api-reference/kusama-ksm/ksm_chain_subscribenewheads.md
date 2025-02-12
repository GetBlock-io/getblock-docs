---
description: >-
  Example code for the chain_subscribeNewHeads  {disallowed} json-rpc method.
  Сomplete guide on how to use chain_subscribeNewHeads  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# chain\_subscribeNewHeads {disallowed} - Kusama

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "chain_subscribeNewHeads",
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
