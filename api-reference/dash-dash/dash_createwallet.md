---
description: >-
  Example code for the createwallet  {disallowed} json-rpc method. Сomplete
  guide on how to use createwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# createwallet {disallowed} - Dash

#### Parameters

`wallet_name` - string

The name for the new wallet. If this is a path, the wallet will be created at the path location.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "createwallet",
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
