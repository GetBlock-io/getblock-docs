---
description: >-
  Example code for the getaddressmempool  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddressmempool  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getaddressmempool {disallowed} - Zcash

#### Parameters

`addresses` - list of string

List of the base58check encoded addresses

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getaddressmempool",
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
