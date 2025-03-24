---
description: >-
  Example code for the net_listening json-rpc method. Сomplete guide on how to
  use net_listening json-rpc in GetBlock.io Web3 documentation.
---

# net\_listening - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "net_listening",
"params": [],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": true
}
```
