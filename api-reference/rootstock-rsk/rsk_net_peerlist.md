---
description: >-
  Example code for the net_peerList  {disallowed} json-rpc method. Сomplete
  guide on how to use net_peerList  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# net\_peerList {disallowed} - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "net_peerList",
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
