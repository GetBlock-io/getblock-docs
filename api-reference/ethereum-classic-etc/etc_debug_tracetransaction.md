---
description: >-
  Example code for the debug_traceTransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use debug_traceTransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# debug\_traceTransaction {disallowed} - Ethereum Classic

#### Parameters

`transactionHash` - data

Transaction hash.

`Object` - None

request options (all optional and default to false)

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_traceTransaction",
"params": [null, null],
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
