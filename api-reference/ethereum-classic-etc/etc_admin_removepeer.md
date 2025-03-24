---
description: >-
  Example code for the admin_removePeer  {disallowed} json-rpc method. Сomplete
  guide on how to use admin_removePeer  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# admin\_removePeer {disallowed} - Ethereum Classic

#### Parameters

`string` - None

Enode URL of peer to remove.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "admin_removePeer",
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
