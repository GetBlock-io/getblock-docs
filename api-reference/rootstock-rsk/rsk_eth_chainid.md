---
description: >-
  Example code for the eth_chainId json-rpc method. Сomplete guide on how to use
  eth_chainId json-rpc in GetBlock.io Web3 documentation.
---

# eth\_chainId - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_chainId",
"params": [],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x1e"
}
```
