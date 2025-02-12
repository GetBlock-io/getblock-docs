---
description: >-
  Example code for the debug_batchSendRawTransaction  {disallowed} json-rpc
  method. Сomplete guide on how to use debug_batchSendRawTransaction 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# debug\_batchSendRawTransaction {disallowed} - Ethereum Classic

#### Parameters

`data` - None

The signed transaction data array.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_batchSendRawTransaction",
"params": [null],
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
