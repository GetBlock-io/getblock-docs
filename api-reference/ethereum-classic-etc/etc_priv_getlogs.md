---
description: >-
  Example code for the priv_getLogs  {disallowed} json-rpc method. Сomplete
  guide on how to use priv_getLogs  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# priv\_getLogs {disallowed} - Ethereum Classic

#### Parameters

`data` - None

32-byte privacy Group ID.

`Object` - None

Filter options object.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "priv_getLogs",
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
