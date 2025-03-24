---
description: >-
  Example code for the clique_getSigners  {disallowed} json-rpc method. Сomplete
  guide on how to use clique_getSigners  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# clique\_getSigners {disallowed} - Ethereum Classic

#### Parameters

`quantity|tag` - None

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "clique_getSigners",
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
