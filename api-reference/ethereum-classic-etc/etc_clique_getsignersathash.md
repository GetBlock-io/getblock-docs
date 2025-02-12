---
description: >-
  Example code for the clique_getSignersAtHash  {disallowed} json-rpc method.
  Сomplete guide on how to use clique_getSignersAtHash  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# clique\_getSignersAtHash {disallowed} - Ethereum Classic

#### Parameters

`data` - None

32-byte block hash.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "clique_getSignersAtHash",
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
