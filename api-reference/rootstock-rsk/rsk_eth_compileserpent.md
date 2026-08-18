---
description: >-
  Example code for the eth_compileSerpent  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_compileSerpent  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_compileSerpent {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_compileSerpent",
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
