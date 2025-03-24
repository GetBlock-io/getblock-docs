---
description: >-
  Example code for the clique_propose  {disallowed} json-rpc method. Сomplete
  guide on how to use clique_propose  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# clique\_propose {disallowed} - Ethereum Classic

#### Parameters

`data` - None

20-byte address.

`boolean` - None

true to propose adding signer or false to propose removing signer.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "clique_propose",
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
