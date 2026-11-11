---
description: >-
  Example code for the eth_mining json-rpc method. Сomplete guide on how to use
  eth_mining json-rpc in GetBlock.io Web3 documentation.
---

# eth\_mining - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_mining",
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
