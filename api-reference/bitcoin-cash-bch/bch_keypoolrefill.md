---
description: >-
  Example code for the keypoolrefill  {disallowed} json-rpc method. Сomplete
  guide on how to use keypoolrefill  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# keypoolrefill {disallowed} - Bitcoin Cash

#### Parameters

`newsize` - numeric, optional, default=100

The new keypool size

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "keypoolrefill",
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
