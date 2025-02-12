---
description: >-
  Example code for the getFeeRateGovernor  {disallowed} json-rpc method.
  Сomplete guide on how to use getFeeRateGovernor  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getFeeRateGovernor {disallowed} - Solana

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://sol.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getFeeRateGovernor",
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
