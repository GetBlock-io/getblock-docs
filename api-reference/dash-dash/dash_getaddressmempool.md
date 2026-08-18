---
description: >-
  Example code for the getaddressmempool  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddressmempool  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getaddressmempool {disallowed} - Dash

#### Parameters

`addresses` - object

An array of P2PKH or P2SH Dash address(es).

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getaddressmempool",
"params": [[{"name": "address", "type": "string (base58)", "description": ["The base58check encoded address."], "value": null}]],
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
