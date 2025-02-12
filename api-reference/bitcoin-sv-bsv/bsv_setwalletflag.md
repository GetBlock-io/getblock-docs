---
description: >-
  Example code for the setwalletflag  {disallowed} json-rpc method. Сomplete
  guide on how to use setwalletflag  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# setwalletflag {disallowed} - Bitcoin SV

#### Parameters

`flag` - string, required

The name of the flag to change. Current available flags - avoid\_reuse

`value` - boolean, optional, default=true

The new state.

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "setwalletflag",
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
