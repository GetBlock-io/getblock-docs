---
description: >-
  Example code for the shh_addToGroup  {disallowed} json-rpc method. Сomplete
  guide on how to use shh_addToGroup  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# shh\_addToGroup {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "shh_addToGroup",
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
