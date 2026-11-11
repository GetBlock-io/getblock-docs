---
description: >-
  Example code for the eth_getCompilers  {disallowed} json-rpc method. Сomplete
  guide on how to use eth_getCompilers  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getCompilers {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getCompilers",
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
