---
description: >-
  Example code for the clique_discard  {disallowed} json-rpc method. Сomplete
  guide on how to use clique_discard  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# clique\_discard {disallowed} - Ethereum Classic

#### Parameters

`data` - None

20-byte address of proposed signer.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "clique_discard",
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
