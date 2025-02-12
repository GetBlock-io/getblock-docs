---
description: >-
  Example code for the getaddressinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddressinfo  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getaddressinfo {disallowed} - Dash

#### Parameters

`Address` - string (base58)

The P2PKH or P2SH address encoded in base58check format

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getaddressinfo",
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
