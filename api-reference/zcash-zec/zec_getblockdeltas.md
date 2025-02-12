---
description: >-
  Example code for the getblockdeltas  {disallowed} json-rpc method. Сomplete
  guide on how to use getblockdeltas  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getblockdeltas {disallowed} - Zcash

#### Parameters

`hash` - string

The block hash.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockdeltas",
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
