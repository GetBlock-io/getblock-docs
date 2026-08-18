---
description: >-
  Example code for the shh_newIdentity  {disallowed} json-rpc method. Сomplete
  guide on how to use shh_newIdentity  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# shh\_newIdentity {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "shh_newIdentity",
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
