---
description: >-
  Example code for the eth_getUncleCountByBlockNumber json-rpc method. Сomplete
  guide on how to use eth_getUncleCountByBlockNumber json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getUncleCountByBlockNumber - Rootstock

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getUncleCountByBlockNumber",
"params": ["latest"],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0"
}
```
