---
description: >-
  Example code for the priv_getPrivateTransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use priv_getPrivateTransaction  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# priv\_getPrivateTransaction {disallowed} - Ethereum Classic

#### Parameters

`data` - None

Transaction hash returned by eea\_sendRawTransaction or eea\_sendTransaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "priv_getPrivateTransaction",
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
