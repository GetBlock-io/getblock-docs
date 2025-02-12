---
description: >-
  Example code for the net_peerCount json-rpc method. Сomplete guide on how to
  use net_peerCount json-rpc in GetBlock.io Web3 documentation.
---

# net\_peerCount - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "net_peerCount",
"params": [],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x20"
}
```
