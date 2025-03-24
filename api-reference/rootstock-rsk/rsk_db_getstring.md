---
description: >-
  Example code for the db_getString  {disallowed} json-rpc method. Сomplete
  guide on how to use db_getString  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# db\_getString {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "db_getString",
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
