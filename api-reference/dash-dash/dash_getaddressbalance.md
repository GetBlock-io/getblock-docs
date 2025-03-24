---
description: >-
  Example code for the getaddressbalance  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddressbalance  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getaddressbalance {disallowed} - Dash

#### Parameters

`addresses` - object

An array of P2PKH or P2SH Dash address(es).

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getaddressbalance",
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
