---
description: >-
  Example code for the eth_newPendingTransactionFilter json-rpc method. Сomplete
  guide on how to use eth_newPendingTransactionFilter json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_newPendingTransactionFilter - Rootstock

#### Parameters

\-

#### Request

```java
wscat -c wss://rsk.getblock.io/YOUR-API-KEY/mainnet/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_newPendingTransactionFilter",
"params": [],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x5e91"
}
```
