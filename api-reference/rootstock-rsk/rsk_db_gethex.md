---
description: >-
  Example code for the db_getHex  {disallowed} json-rpc method. Сomplete guide
  on how to use db_getHex  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# db\_getHex {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "db_getHex",
"params": [],
"id": "getblock.io"}
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
