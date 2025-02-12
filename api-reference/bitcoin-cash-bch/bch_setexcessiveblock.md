---
description: >-
  Example code for the setexcessiveblock  {disallowed} json-rpc method. Сomplete
  guide on how to use setexcessiveblock  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# setexcessiveblock {disallowed} - Bitcoin Cash

#### Parameters

`maxBlockSize` - numeric, required

Excessive block size in bytes. Must be greater than 1000000.

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "setexcessiveblock",
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
