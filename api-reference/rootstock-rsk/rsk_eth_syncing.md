---
description: >-
  Example code for the eth_syncing json-rpc method. Сomplete guide on how to use
  eth_syncing json-rpc in GetBlock.io Web3 documentation.
---

# eth\_syncing - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_syncing",
"params": [],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": false
}
```
