---
description: >-
  Example code for the getaddressutxos  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddressutxos  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getaddressutxos {disallowed} - Dash

#### Parameters

`addresses` - object

An array of P2PKH or P2SH Dash address(es).

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getaddressutxos",
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
