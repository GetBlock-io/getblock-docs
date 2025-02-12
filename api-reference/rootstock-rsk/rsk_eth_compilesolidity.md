---
description: >-
  Example code for the eth_compileSolidity  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_compileSolidity  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_compileSolidity {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_compileSolidity",
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
