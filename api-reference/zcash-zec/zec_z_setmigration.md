---
description: >-
  Example code for the z_setmigration  {disallowed} json-rpc method. Сomplete
  guide on how to use z_setmigration  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# z\_setmigration {disallowed} - Zcash

#### Parameters

`enabled` - boolean

"true" or "false" to enable of disable respectively.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "z_setmigration",
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
